## Inheritance in JavaScript

Because of the way JavaScript works, with the ***prototype chain***, the sharing of functionality between objects is often called **delegation**. Specialized objects delegate functionality to a generic object type.

The following shows the lookup of `propery` through along the **prototype chain**.
![property_lookup](./assets/property_lookup.png)

In contrast, the lookup(or resolution) of `identifier` is through the **scope chain**.

## ES5-style inheritance

![es5_style_inheritance](./assets/es5_style_inheritance.png)


## ES6-style inheritance

![es6_style_inheritance](./assets/es6_style_inheritance.png)

> Note that the inner **[[Class]]** property of instance may not set to SubClass or SuperClass, the result is dependent on wether the `constructor` ***SubClass*** or ***SuperClass*** is builtin functio nor not.

## Difference 

The main important difference of inheritance between es5-style and es6-style is the truely creator of the instance. In es5-style, the creator is the ***SubClass***, while in es6-style the ***SuperClass*** is the truely creator. 

Take builtin function Date as an example:
```javascript
// es5-style 
function CustomDate() {
   this.prop = "value";
   // no return here
}
Object.setPrototypeOf(CustomDate.prototype, Date.prototype);

var date = new CustomDate()
Object.prototype.toString.call(date) // "[object Object]"
```
For ***CustomDate***, the **SubClass**, is not builtin function, so the inner `[[Class]]` property of the newly-created instance is set to **Object**. 

```javascript
// es6-style with `class` syntax sugar
class CustomDate extends Date {
    constructor() {
        super();
        this.prop = "value";
    }
}

var date = new CustomDate()
Object.prototype.toString.call(date) // "[object Date]"

// es6-style without `class` syntax sugar
function CustomDate() {
    
    var dateInst = new (Function.prototype.bind.apply(Date, [Date].concat(Array.prototype.slice.call(arguments))))();

    Object.setPrototypeOf(dateInst, CustomDate.prototype);

    return dateInst;
}
Object.setPrototypeOf(CustomDate.prototype, Date.prototype);

var date = new CustomDate()
Object.prototype.toString.call(date) // "[object Date]"
```
Since the ***Date*** is a builtin, the inner `[[Class]]` property of the newly-created instance is set to **Date**. 
