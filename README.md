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
  - Another example

  ```javascript
  var makeArray = function(){
    return [];
  };
var array1 = makeArray();
var array2 = makeArray();
log(array1 === array2); => false
```
  - array1 and array2 are two seperate objects, so they fail a triple equals comparison

##The Parameter 'this'
  - `this` is an identifier that gets bound to the correct object automatically

  ```javascript
  var fn = function(a,b){
    log(this)
  };
```

  - `this` refers to the object that fn belongs to
    - in others words `obj.fn(a,b);` the `obj` found to the left of the dot (the `obj` that `fn` belongs to) is `this`
###Example
    ____________

  - input parameters to a function only have binding when that function is actually running

  ```javascript
  var fn = function(one, two){
    log(one, two);
  };
  ```

  In the above example, neither one nor two are bound to an object. It's not until `fn` is actually executed that they become bound.

  ```javascript
  var fn = function(one, two){
    log(one, two);
  };
  var r={}, g={}, b={};

  fn(g,b);
  ```

  ```javascript
  var fn = function(one, two){
    log(this, one, two);
  };
  var r={}, g={}, b={};
  r.method = fn;

  r.method(g,b);
  ```

  Here `this` refers to the **value** of `r`, which is `{}`

  ____________

  **Q** What if you want `this` to refer to an object that does not have the function it is invoked within as a property of it?

  **A** You can invoke the `call` method

  - This will allow you to pass in any value you want `this` to refer to
  - Note: the use of `call` overrides the dot access rule, so for instance

  ```javascript
  var fn = function(one, two){
    log(this, one, two);
  };
  var r={}, g={}, b={}, y={};

  r.method.call(y,g,b);
  ```

  Here, even though the object `r` is found during the invocation of `method`, the `call` method overrides the `r` object - `this` will now refer to `y`

#Prototype Chains
  - a mechanism for creating objects that resemble other objects

```javascript
  var gold = {a:1};
  var rose = Object.create(gold);
  robe.b = 2;
  log(rose.a);
  ```

  This creates an **ongoing** lookup on the rose object that will *fall back* on the object it inherits from.
  - There exists a top level object that every Javascript object eventually delegates to.
    - This is called the *object prototype*
    - This object has methods available to it, and is the reason you can call those methods on newly created objects
    - an example is `newThing.toString();` - this has a toString method because the object prototype has this method defined on it
    - Because of the way `this` works, the `toString` method works as you would expect it to with the *dot rule*
      - Even though `toString` belongs to the object prototype originally, `newThing.toString();` with have `this` refer to `newThing`
    - There are other prototypes that behave differently than the object prototype, and are used by certain object types
      - Arrays have an array prototype, that gives you access to methods like `indexOf();`
