# Type checking

This is one of the sad things about Javascript, not only are there many varying implementations of the language, there are also many varying opinions about how things should be done.

## typeof operator

- `typeof` is pretty good at distinguishing between different kind of primitive values, and distinguish between them and objects.
-  completely useless when it comes to distinguishing between different kinds of objects.

## instanceof operator

The `instanceof` operator tells you whether a object is an instance of a certain type. The so-called "type" is a constructor -- **a Function object**.

The production RelationalExpression: ***RelationalExpression `instanceof `ShiftExpression*** is evaluated as follows:
1. Let ***lref*** be the result of evaluating ***RelationalExpression***.
2. Let ***lval*** be GetValue(***lref***).
3. Let ***rref*** be the result of evaluating ***ShiftExpression***.
4. Let ***rval*** be GetValue(***rref***).
5. If Type(***rval***) is not Object, throw a TypeError exception.
6. If ***rval*** does not have a [[HasInstance]] internal method, throw a TypeError exception.
7. Return the result of calling the [[HasInstance]] internal method of ***rval*** with argument ***lval***.
> only Function objects implement [[HasInstance]].

Assume ***F*** is a Function object, when the [[HasInstance]] internal method of ***F*** is called with value ***V***, the following steps are taken:
1. If ***V*** is not an object, return false.
2. Let ***O*** be the result of calling the [[Get]] internal method of F with property name "***prototype***".
3. If Type(***O***) is not Object, throw a TypeError exception.
4. Repeat
	a. Let ***V*** be the value of the [[Prototype]] internal property of ***V***.
	b. If ***V*** is null, return false.
	c. If ***O*** and ***V*** refer to the same object, return true.

#### #1 Not work for primitives
But  the `instanceof` does not work with primitive values. The primitive types in Javascript are: strings, numbers, booleans, null, and undefined. 
```javascript
3 instanceof Number // false
true instanceof Boolean // false
'abc' instanceof String // false
```
> null and undefined are not an instance of anything

Checking for the `constructor` property will work for the primitive types number, string and boolean:
```javascript
(3).constructor === Number // true
true.constructor === Boolean // true
'abc'.constructor === String // true
```
This works because whenever you reference a property on a primitive value, Javascript will automatically wrap the value with an object wrapper.

#### #2 Cross-window Issues

An object created within the iframe is only an instance of the corresponding constructor within that iframe.
```javascript
var iframe = document.createElement('iframe')
document.body.appendChild(iframe)
var iWindow = iframe.contentWindow // get a reference to the window object of the iframe
iWindow.document.write('<script>var obj = {name: "Gyumeijie"} </script>') // create an obj in iframe's window
iWindow.obj // {name: "Gyumeijie"}
iWindow.obj instanceof Object // false
```

```javascript
Object === iWindow.Object // false
iWindow.obj instanceof iWindow.Object // true
```

## Duck-Typing

This means checking the ***behavior***:  if it looks like a duck and quacks like a duck, then it is a duck as far as I am concerned.

So, using duck-typing, an isArray check might look like
```javascript
function isArray(obj){
    return (typeof(obj.length)=="undefined") ?
        false:true;
}
```

in jQuery, the function to check whether an object is a window is
```javascript
isWindow: function( obj ) {
    return obj && typeof obj === "object" && "setInterval" in obj;
}
```
but You could easily trick this function into returning true
```javascript
$.isWindow({setInterval: 'bah!'}) // true
```
Clearly, the problem with this approach is that
1. it is inexact and can have false positives. ( **false positives**)
2. the set of properties of the object to test is arbitrary, and so it's unlikely for different people to agree on one way of doing it.   (**inconsistency**)


## Object.prototype.toString method(only native types)

This method reliably differentiates between native types, however, will return "[object Object]" for all user-defined types.

For a object of user-defined type, whether it is created with object literal or constructed with `new`, it's [[Class]] internal property is set to "Object".


## Function.prototype.toString method(both native and user-defined types)

The method gives you the `complete source of a function`. In the case of native functions, it just says "[native code]" in the body. One could easily parse out the name of the function to figure out type of the object using this helper function:
```	javascript
function type(obj){
	var text = Function.prototype.toString.call(obj.constructor)
	return text.match(/function (.*)\(/)[1]
}
```

But with this approach, there's no way to distinguish between two different user-defined types with the same name: 

```javascript
var f = function Animal(){ "something" }
var g = function Animal(){ "something entirely different" }

type(new f) // "Animal"
type(new g) // "Animal"
```
For this reason, this method is inferior to `instanceof` for user-defined types. 
