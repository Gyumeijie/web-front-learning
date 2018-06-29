# Ch1 Types

## Built-in Types
JavaScript defines seven built-in types:

- null
- undefined
- boolean
- number
- string
- object
- symbol -- added in ES6!

> Note: All of these types except **object** are called "**primitives**".

## The typeof operator
The `typeof` return a result of string type as shown the following table: 

Type of val  |  Result
------------- | ---------
Undefined | "undefined"
Null | "object"
Boolean | "boolean"
Number | "number"
String | "string"
Symbol | "symbol"
Object (ordinary and does not implement [[Call]])	| "object"
Object (standard exotic and does not implement [[Call]]) | "object"
Object (implements [[Call]]) | "function"
Object (non-standard exotic and does not implement [[Call]])	| Implementation-defined. Must not be "undefined", "boolean",  "function", "number", "symbol", or "string".

> Implementations are discouraged from defining new typeof result values for non-standard exotic objects. If possible "object" should be used for such objects.
> 
> The typeof operator isn't guaranteed to return consistent results across browsers.

## Values as Types
In JavaScript, variables don't have types -- values have types, the same as `python`. Variables can hold any value, at any time.


## undefined vs undeclared
- An "undefined" variable is one that has been declared in the accessible scope, but at the moment has no other value in it. 
- An "undeclared" variable is one that has not been formally declared in the accessible scope.

##### Check against global global variables
- window.globalVariableName
- the safety guard feature of typeof: typeof globalVariableName

> The `typeof Undeclared` returns "undefined" but doesn't throw a ReferenceError



# Ch2 Values

## Arrays
As compared to other type-enforced languages, JavaScript arrays are just **containers for any type of value**, from string to number to object to even another array (which is how you get multidimensional arrays).
#### Array-Likes to Array
- Array.prototype.slice.call( array-like )
- Array.from( array-like) `ES6`

## Numbers
JavaScript has just one numeric type: number, and the implementation of JavaScript's numbers is based on the "IEEE 754" standard, often called "floating-point." 


## Value vs. Reference
In many other languages, values can either be assigned/passed by value-copy or by reference-copy depending on the syntax you use.

- Simple values (a.k.a scalar primitives) are always assigned/passed by value-copy:null, undefined, string, number, boolean, and ES6's symbol.
- Compound values -- objects (including arrays, and all **boxed object wrappers** ) and functions -- always create a copy of the reference on assignment or passing.

> References are not like references/pointers in other languages -- they're never pointed at other variables/references, only at the underlying values.



# Ch3 Natives

Here's a list of the most commonly used natives:

- String()
- Number()
- Boolean()
- Array()
- Object()
- Function()
- RegExp()
- Date()
- Error()
- Symbol() -- added in ES6!
As you can see, these natives are actually **built-in functions**.

## Internal [[class]]
- The value of the [[Class]] internal property is defined by this specification for every kind of built-in object. 
-  The value of the [[Class]] internal property of a host object may be any String value except one of "Arguments", "Array", "Boolean", "Date", "Error", "Function", "JSON", "Math", "Number", "Object", "RegExp", and "String". 
- The value of a [[Class]] internal property is used internally to distinguish different kinds of objects. 

The JSON object is a single object that contains two functions, parse and stringify, that are used to parse and construct JSON texts. 
```javascript
Object.prototype.toString.call(JSON)  // "[object JSON]"
```
The Arguments Object.
```javascript
Object.prototype.toString.call(arguments) // "[object Arguments]"
```


Note that this specification does not provide any means for a program to access that value except **through Object.prototype.toString**, when the toString method is called, the following steps are taken:
1. If the this value is undefined, return "[object Undefined]".
2. If the this value is null, return "[object Null]".
3. Let O be the result of calling ToObject passing the this value as the argument.
4. Let `class` be the value of the `[[Class]]` internal property of O.
5. Return the String value that is the result of concatenating the three Strings "[object", `class`, and "]".

## Boxing Wrappers
These object wrappers serve a very important purpose. Primitive values don't have properties or methods, so to access .length or .toString() you need an object wrapper around the value. 

Thankfully, JS will automatically box (a.k.a wraps it in its respective object wrapper) the primitive value to fulfill such accesses.

> Once the primitive value is boxed, the wrapped value is an object, so the following snippet
> does't work: 
```javascript
var a = new Boolean( false );
f (!a) {
		console.log( "Oops" ); // never runs
}
```
 Note that there is a `new` keyword.

## Unboxing
If you have an object wrapper and you want to get the underlying primitive value out, you can use the `valueOf()` method.




# Ch4 Coercion

## Converting Values
- **type casting**, occur in statically typed languages at compile time.
- **type coercion**,  a runtime conversion for dynamically typed languages.

>  JavaScript coercions always result in one of the scalar primitive values, like string, number, or boolean. There is no coercion that results in a complex value like object or function.

#### ToPrimitive Conversions
The abstract operation ToPrimitive takes an `input` argument and an optional argument `PreferredType`. The abstract operation ToPrimitive converts its input argument to a non-Object type. 

If an object is capable of converting to more than one primitive type, it may use the optional `hint PreferredType` to favour that type. 

The internal operation ToPrimitive() has the following signature:
> ToPrimitive(input, PreferredType?)

The optional parameter PreferredType is either ***Number*** or ***String***. PreferredType is omitted and thus Number for **non-dates**, String for **dates**. :star: :star:



Input Type |  Result
---- | ---
Undefined	| Return input.
Null	| Return input.
Boolean | Return input.
Number	| Return input.
String	| Return input.
Symbol	| Return input.
Object	| Perform the steps following this table.

> The default `PreferredType` was "Number".

- ToPrimitive(obj, Number): 
	- To convert an object obj to a primitive, invoke obj.valueOf(). If the result is primitive, return that result. 
	- Otherwise, invoke obj.toString(). If the result is primitive, return that result. 
	- Otherwise, throw a TypeError.
- ToPrimitive(obj, String): Works the same, but invokes obj.toString() before obj.valueOf().


## Explicit Coercion

- To string
   - String(value) without `new`
   - "" + value
   -  value.toString()
- To  number
	- Number(value) without `new`
	- parseInt(value)
	- parseFloat(value) 
	- `+` operator
- To boolean
   - Boolean(value) without `new`
   - `!!` operator

## Loose Equals vs. Strict Equals
`== `allows coercion in the equality comparison and `===` disallows coercion.

Some minor exceptions to normal expectation to be aware of:
- `NaN `is never equal to itself (see Chapter 2)
- `+0` and `-0` are equal to each other (see Chapter 2)
