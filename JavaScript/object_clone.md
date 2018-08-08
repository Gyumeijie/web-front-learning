# Object cloning in Javascript
There are two different ways to clone an object in Javascript, shallow and deep copy.

## Shallow copy
Shallow copy is a **bit-wise copy** of an object. 

A new object is created that has an exact copy of the values in the original object. If any of the fields of the object are references to other objects, just the reference addresses are copied i.e., :star: **only the memory address is copied**.

## Deep copy
A deep copy copies all fields, and makes copies of :star: **dynamically allocated memory** pointed to by the fields. A deep copy occurs when an object is copied along with the objects to which it refers.

![](https://github.com/Gyumeijie/assets/blob/master/web-front-learning/shallow_and_deep_clone.png)

# How to copy JavaScript object to new variable NOT by reference

- For simple JSON objects
```javascript
var objectIsNew = JSON.parse(JSON.stringify(objectIsOld));
```

In Jquery you can use:
```javascript
// Shallow copy
var objectIsNew = jQuery.extend({}, objectIsOld);

// Deep copy
var objectIsNew = jQuery.extend(true, {}, objectIsOld);
```
> In this way the ***Function object properties*** and ***properties in prototype*** cann't be cloned, for more information please refer to [stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).

- Pure javascript method to deep clone object 

![](https://github.com/Gyumeijie/assets/blob/master/web-front-learning/object_clone.png)

```javascript
function deepCopy(obj, hash = new WeakMap()) {
   
   if (Object(obj) !== obj || obj instanceof Function) return obj  // Do not try to clone primitives or functions
   if (hash.has(obj)) return hash.get(obj) // Cyclic reference

   var properties = new obj.constructor() // Register in hash  
   hash.set(obj, properties)

   return Object.assign(properties, ...Object.keys(obj).map(function(key) {
        return {[key]: deepCopy(obj[key], hash)}
   }))
}
```
> Note: use ```var properties = new obj.constructor()``` instead of  ```var properties = {}``` to register in hash

Optionally, we can add support for some standard constructors (extend as desired) 
```javascript
function deepCopy(obj, hash = new WeakMap()) {
   
   if (Object(obj) !== obj || obj instanceof Function) return obj  // Do not try to clone primitives or functions
   if (hash.has(obj)) return hash.get(obj) // Cyclic reference
   
   var properties = new obj.constructor() // Register in hash  
   hash.set(obj, properties)
   
   if (obj instanceof Map) {
       Array.from(obj, ([key, val]) => properties.set(deepClone(key, hash), deepClone(val, hash)))
   } else if (obj instanceof Set) {
       Array.from(obj, (key) => properties.add(deepClone(key, hash)))
   } else { 
       return Object.assign(properties, ...Object.keys(obj).map(function(key) {
            return {[key]: deepCopy(obj[key], hash)} 
       }))
   }
}
```

Note that ***Map*** and ***Set*** cann't use `Object.keys()` method, take the following snippet as an example:
```javascript
var map = new Map()
people = {name: "YMJ", age: 25}
map.set(people, "YuMeiJie")

Object.keys(map) // []
```


