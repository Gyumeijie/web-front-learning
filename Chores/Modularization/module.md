# Why use modules?

- Maintainability
By definition, a module is ***self-contained***. A well-designed module aims to ***reduce the dependencies*** on parts of the codebase as much as possible, so that it can grow and improve independently. Updating a single module is much easier when the module is ***decoupled*** from other pieces of code.

- Namespacing
In JavaScript, variables outside the scope of a top-level function are global. it’s common to have ***namespace pollution***, where completely unrelated code shares global variables.

- Reusability


# Module patterns

## Anonymous closure

```javascript

(function () {
  
  // We keep these variables private inside this closure scope
  var localVariable1, localVariable2;
  
  // Define functions
  var function1 = function() {
       // access local variables
  }
  var function2 = function() {
       // access local variables
  }

  // Call functions
  function1();
  function2();
}());
```

With this construct, our anonymous function has its own evaluation environment,this lets us hide variables from the parent (global) namespace, ***avoiding namespace pollution***.

What’s nice about this approach is that is that you can use local variables inside this function without accidentally overwriting existing global variables, yet still access the global variables, like so:

```javascript

var globalVariable1, globalVariable2;

(function () {
  
  // We keep these variables private inside this closure scope
  var localVariable1, localVariable2;
  
  // Define functions
  var function1 = function() {
       // access local or global variables
  }
  var function2 = function() {
       // access local or global variables
  }

  // Call functions
  function1();
  function2();
}());
```

## Global import

Another popular approach used by libraries like jQuery is global import. It’s similar to the anonymous closure we just saw, except now we pass in globals as parameters:

```javascript
(function (globalVariable1, globalVariable2) {
  
  // We keep these variables private inside this closure scope
  var localVariable1, localVariable2;
  
  // Define functions
  var function1 = function() {
       // access local or global variables
  }
  var function2 = function() {
       // access local or global variables
  }

  // Call functions
  function1();
  function2();
}(globalVariable1, globalVariable2));
```
The benefit of this approach over anonymous closures is that you declare the global variables explictly, making it crystal clear to people reading your code, and thus enhance readability.

## Object interface

This approach is to create modules using a self-contained object interface, like so:

```javascript
var objectInterface = (function () {
  
  // We keep these variables private inside this closure scope
  var localVariable1, localVariable2;
  
  // Expose these functions via an interface while hiding
  // the implementation of the module within the function() block
  return {
       function1 : function() {
         // access local variables
       },
       function2 : function() {
         // access local variables
       }
  }
}());
```

In this approach, we can us decide what variables/methods we want to keep ***private*** and what variables/methods we want to ***expose*** by putting them in the return statement.


## Revealing module pattern
This is very similar to the above approach, except that it ensures all methods and variables are kept private until explicitly exposed:

```javascript
var objectInterface = (function () {
  
  // We keep these variables private inside this closure scope
  var localVariable1, localVariable2;
  
  var function1 = function() {
       // access local variables
  }
  var function2 = function() {
       // access local variables
  }

  return {
      function1: function1,
      function2: function2
  }
}());
```

## CommonJS

A CommonJS module is essentially a reusable piece of JavaScript which exports specific objects, making them available for other modules to ***require*** in their programs.

With CommonJS, each JavaScript file stores modules in its own unique module context (just like wrapping it in a closure). In this scope, we use the `module.exports object` to ***expose*** modules, and `require` to ***import*** them.

```javascript
function myModule() {
  this.hello = function() {
    return 'hello!';
  }

  this.goodbye = function() {
    return 'goodbye!';
  }
}

module.exports = myModule;
```

Then when someone wants to use myModule, they can require it in their file, like so:

```javascript
var myModule = require('myModule');

var myModuleInstance = new myModule();
myModuleInstance.hello(); // 'hello!'
myModuleInstance.goodbye(); // 'goodbye!'
```

There are two obvious benefits to this approach over the module patterns we discussed before:
- Avoiding global namespace pollution
- Making our dependencies explicit

Another thing to note is that CommonJS takes a ***server-first*** approach and ***synchronously loads modules***. 

## AMD

Asynchronous Module Definition. Loading modules using AMD looks something like this:

```javascript
define(['myModule', 'myOtherModule'], function(myModule, myOtherModule) {
  console.log(myModule.hello());
});
```

The define function takes as its first argument an array of each of the module’s dependencies. Next, the callback function takes, as arguments, the dependencies that were loaded — in our case, myModule and myOtherModule — allowing the function to use these dependencies.

Unlike CommonJS, AMD takes a ***browser-first*** approach alongside ***asynchronous*** behavior to get the job done.

AMD isn’t compatible with io, filesystem, and other server-oriented features available via CommonJS.

## UMD

For projects that require you to support both AMD and CommonJS features, there’s yet another format: Universal Module Definition (UMD).

UMD essentially creates a way to use either of the two, while also supporting the global variable definition. As a result, UMD modules are capable of working on ***both client and server***.

```javascript
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
      // AMD
    define(['module1', 'module2'], factory);
  } else if (typeof exports === 'object') {
      // CommonJS
    module.exports = factory(require('module1'), require('module2'));
  } else {
    // Browser globals (Note: root is window)
    root.returnExports = factory(root.module1, root.module2);
  }
}(this, function (module1, module2) {
  
  // Methods
  function function1(){}; // A private method
  function function2(){}; // A public method because it's returned (see below)
  function function3(){}; // A public method because it's returned (see below)

  // Exposed public methods
  return {
      function1: function1,
      function2: function2,
  }
}));
```
