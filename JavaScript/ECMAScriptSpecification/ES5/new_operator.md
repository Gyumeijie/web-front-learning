## The new Operator

The production MemberExpression : new MemberExpression Arguments is evaluated as follows:
1. Let ***ref*** be the result of evaluating ***NewExpression***.
> When evaluating ***NewExpression*** a **Reference** is created.

2. Let ***constructor*** be GetValue(***ref***).
> The ***base*** value of ***ref*** may be an ***Object*** or a ***Environment Record***.
> - If the ***base*** value equals to ***Object***, then return the result of calling the ***get*** internal method 
using ***base*** as its **this** value, and passing ***GetReferencedName(ref)*** for the argument.
> - ***base*** must be an ***Environment Record***, then return the result of calling the ***GetBindingValue*** concrete 
method of base passing ***GetReferencedName(V)*** and ***IsStrictReference(V)*** as arguments.

3. Let ***argList*** be the result of evaluating ***Arguments***, producing an internal list of argument values.

4. If Type(***constructor***) is not Object, throw a TypeError exception.
5. If ***constructor*** does not implement the ***[[Construct]]*** internal method, throw a TypeError exception.
> steps 4 and 5: ensures that the constructor is an function, the ***[[Construct]]*** internal property is only defined in Function Object.

6. Return the result of calling the ***[[Construct]]*** internal method on ***constructor***, providing the list ***argList*** as the argument values.

#### #1 [[Construct]]

When the ***[[Construct]]*** internal method for a Function object ***F*** is called with a possibly empty list of ***arguments***,
the following steps are taken:

1. Let ***obj*** be a newly created native ECMAScript object.
2. Set all the internal methods of ***obj***.
3. Set the ***[[Class]]*** internal property of obj to "***Object***".
> Steps 1, 2 and 3: Created a new Object as ***instance***.
> 
> The ***[[Class]]*** of the object created by built-in Function such as Date, Error is set to a corresponding special value for example "Date", "Error".

4. Set the [[Extensible]] internal property of obj to true.
5. Let ***proto*** be the value of calling the ***[[Get]]*** internal property of ***F*** with argument "***prototype***".
6. If Type(***proto***) is Object, set the ***[[Prototype]]*** internal property of ***obj*** to ***proto***.
7. If Type(***proto***) is not Object, set the ***[[Prototype]]*** internal property of ***obj*** to the standard built-in Object prototype object.
> Steps 5, 6 and 7: set [[Prototype]] internal property of ***obj***.

8. Let ***result*** be the result of calling the ***[[Call]]*** internal property of ***F***, providing ***obj*** as the ***this*** value and providing the argument ***list*** passed into ***[[Construct]]*** as args.
> Step 8: calling the constructor ***F***.
> 
> Functions in ES5 are dual-purpose: can be called with or without ***new*** operator.
> - If a function ***f*** called with ***new***, then the ***[[Construct]]*** internal `method` of ***f*** is called first and then the ***[[Call]]*** internal `method`.
> - Otherwise, only the ***[[Call]]*** internal `method` of ***f*** is called.

9. If Type(***result***) is Object then return result.
```javascript
function Person(name, age){  
   this.name = name;
   this.age = age;
   
   return {property: "for test purpose only."}
}
p = new Person("YuMeiJie", 25);
// return {property: "for test purpose only."}
```
10. Return obj.
```javascript
function Person(name, age){  
   this.name = name;
   this.age = age;
}
p = new Person("YuMeiJie", 25);
// return Person {name: "YuMeiJie", age: 25}
```

#### #2 [[call]]

When the ***[[Call]]*** internal method for a Function object ***F*** is called with a ***this*** value and a list of ***arguments***, the following steps are taken:

1. Let ***funcCtx*** be the result of establishing a new execution context for function code using the value of ***F***'s ***[[FormalParameters]]*** internal property, the passed arguments List ***args***, and the ***this*** value
> Step 1: initialize environment.
> - Establish a new execution context.
> - Enter function code and perform ***Declaration Binding Instantiation***(a.k.a [hoisting](../../hoisting.md)).

2. Let ***result*** be the result of evaluating the ***FunctionBody*** that is the value of ***F***'s ***[[Code]]*** internal property. If ***F*** does not have a ***[[Code]]*** internal property or if its value is an empty ***FunctionBody***, then ***result*** is (**normal**, undefined, empty). 
> Step 2: evaluate function body.
> The ***result*** is type of [Completion](./es_types.md).

3. Exit the execution context ***funcCtx***, restoring the previous execution context.
> Step 3: restore previous execution context.

4. If **result**.type is **throw** then throw result.value.
5. If **result**.type is **return** then return result.value.
6. Otherwise **result**.type must be **normal**. Return undefined.
> Steps 4, 5 and 6: return the the result of evaluating the function body. 
> 
> Of all the types of Completion, only the **throw**, **return** and **normal**  cause the exit of the function context.
