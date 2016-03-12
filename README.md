## Object Oriented Javascript
  - Udacity course on OO JS

#NOTES
##Lexical Scope
  - In some JS environments, global scope is shared across different files
    - if you add a variable to the global scope, it is available within the entire program
    - you can create a lexical scope by adding a function
    ```javascript
    var myFunction = function() {
      var lexicallyScopedVariable = 'foo'
    };
```
    - you can still access variables defined outside the lexical scope of the function
    - you cannot access variables defined within the lexical scope of the function outside of its lexical scope
      - for instance, the following will cause an error
    ```javascript
    var myFunction = function() {
      var lexicallyScopedVariable = 'foo'
    };
    console.log(lexicallyScopedVariable)
```
    - Variables declared outside the function are available within the function, even the function itself
    ```javascript
    var foo = someFunction();
    var myFunction = function() {
      var lexicallyScopedVariable = 'foo';
      console.log(myFunction)
      console.log(foo)
      console.log(lexicallyScopedVariable)
    };
```
    - You can even reassign variables that have not been declared yet
    ```javascript
    var foo = someFunction();
    var myFunction = function() {
      lexicallyScopedVariable = aNewReturnValue();
    };
```
    - note that when doing this, these variables that are reassigned are available in the global scope

  - On lexical scope, only the curly braces found in a function statement create a new scope. Those on, say, the block of an if statement DO NOT create a new lexical scope.
  ```javascript
  var test = aTest();
  if ( checkSomething() ){
    var thisThing = aThing();
  }
log(thisThing);
```
  - This will not cause an error. thisThing was declared within the block of the if statement - which itself does not create a new scope. thisThing is available to the global scope.

##Execution Contexts vs Lexical Scope
  - Lexical scoping is used to declare scope within the context of the code that you write
  - Execution Contexts are in-memory data stores that hold key/value pairs
    - Even though the keys in the execution context may be identical to those that you declare within your code, they are handled differently by the interpreter
      - Where you could loop over the objects declared within a given scope, you cannot do the same with execution contexts.
  - Say for instance you have the following

  ```javascript
  var foo = someFunction();
  var myFunction = function() {
    var lexicallyScopedVariable = anotherFunction();
    var yetAnotherFunction = function(){
      var innerVariable = testFunction();
      log(innerVariable+lexicallyScopedVariable+foo);
    };
  };
```
  - By the time the interpreter gets to the log function, there are 3 execution contexts
    - It will start in the current (most nested) execution context and search outword until it finds values for the variables being referenced
