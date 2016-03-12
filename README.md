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
