# Module formats

A module format is the ***syntax*** we can use to define a ***module***.

Before EcmaScript 6 or ES2015, JavaScript did not have an official syntax to define modules. Therefore, developers came up with various formats to define modules in JavaScript.

Some of the most widely adapted and well known **formats** are:

- Asynchronous Module Definition (AMD)
- CommonJS
- Universal Module Definition (UMD)
- System.register
- ES6 module format


### Asynchronous Module Definition (AMD)

The AMD format is used in browsers and uses a define function to define modules:

```javascript
//Calling define with a dependency array and a factory function
define(['dep1', 'dep2'], function (dep1, dep2) {

    //Define the module value by returning a value.
    return function () {};
});
```

### CommonJS format

The CommonJS format is used in Node.js and uses require and module.exports to define dependencies and modules:

```javascript
var dep1 = require('./dep1');  
var dep2 = require('./dep2');

module.exports = function(){  
  // ...
}
```

### Universal Module Definition (UMD)

The UMD format can be used both in the browser and in Node.js.

```javascript
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    // AMD. Register as an anonymous module.
      define(['b'], factory);
  } else if (typeof module === 'object' && module.exports) {
    // Node. Does not work with strict CommonJS, but
    // only CommonJS-like environments that support module.exports,
    // like Node.
    module.exports = factory(require('b'));
  } else {
    // Browser globals (root is window)
    root.returnExports = factory(root.b);
  }
}(this, function (b) {
  //use b in some fashion.

  // Just return a value to define the module export.
  // This example returns an object, but the module
  // can return a function as the exported value.
  return {};
}));
```

### System.register

The System.register format was designed to support the ES6 module syntax in ES5:

```javascript
import { p as q } from './dep';

var s = 'local';

export function func() {  
  return q;
}

export class C {  
}
```

### ES6 module format

As of ES6, JavaScript also supports a native module format. It uses an export token to export a module's public API:

```javascript
// lib.js

// Export the function
export function sayHello(){  
  console.log('Hello');
}

// Do not export the function
function somePrivateFunction(){  
  // ...

```

and an import token to import parts that a module exports:


```javascript
import { sayHello } from './lib';

sayHello();  
// => Hello

```

Unfortunately, the native module format is not yet supported by all browsers.

We can already use the ES6 module format today, but we need a transpiler like ***Babel*** to transpile our code to an ES5 module format such as AMD or CommonJS before we can actually run our code in the browser.

