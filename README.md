## Object Oriented Javascript
  - Udacity course on OO JS

#NOTES
##Lexical Scope
  - In some JS environments, global scope is shared across different files
    - if you add a variable to the global scope, it is available within the entire program
    - you can create a lexical scope by adding a function
    ``javascript
    var myFunction = function() {
      var lexicallyScopedVariable = 'foo'
    }
```
