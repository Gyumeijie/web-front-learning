## Introduction
 
 This article will introduce the execution context, the flow of entering the function code and how the `this` is bound.

## The Execution Context

When control is transferred to ECMAScript executable code, control is entering an execution context. **Active execution contexts logically form a stack**. The top execution context on this logical stack is the **running execution context**. A new execution context is created whenever control is transferred from the executable code associated with the currently running execution context to executable code that is not associated with that execution context. The newly created execution context is pushed onto the stack and becomes the running execution context.
> **Evaluation** of `global code` or code using the `eval` function establishes and enters a new execution context. Every **invocation** of an ECMAScript code `function` also establishes and enters a new execution context, even if a function is calling itself recursively. 

The execution context includes three state components: 

Component | Purpose
--- | ---
LexicalEnvironment | Identifies the Lexical Environment used to **resolve identifier references** made by code within this execution context.
VariableEnvironment | Identifies the Lexical Environment whose environment record holds bindings created by **VariableStatements** and **FunctionDeclarations** within this execution context.
ThisBinding | The value associated with the **`this`** keyword within ECMAScript code associated with this execution context.

As shown the following snippet, the first `this` evaluates to the value of the `ThisBinding` of the execution context which established when calling the `outer`   and the same as the second `this` .
>  The `this` keyword evaluates to the value of the `ThisBinding` of the current execution context.

```javascript
 function outer() {
   console.log(this); // the first `this`

   function inner() {
      console.log(this); // the second `this`
   }

   return inner;
}
```


Before entering the function code, we have several means to provide the value of `thisArg`, for example:
```javascript
Function.prototype.apply(thisArg, argArray)
Function.prototype.call(thisArg [,arg1 [, arg2, … ]]) 
Function.prototype.bind(thisArg [,arg1 [, arg2, …]])

Array.prototype.every(callbackfn [, thisArg])
Array.prototype.forEach(callbackfn [, thisArg])
// ......
```

When control enters an execution context, the execution context’s **ThisBinding** is set, its **VariableEnvironment** and initial **LexicalEnvironment** are defined, and **declaration binding instantiation** is performed.

## Entering Function Code

The following steps are performed when control enters the execution context for function code contained in function object ***F***, a caller provided ***thisArg***, and a caller provided ***argumentsList***:
1. If the function code is **strict code**, set the `ThisBinding` to ***thisArg***.
2. Else if ***thisArg*** is **null** or **undefined**, set the `ThisBinding` to the **global object**.
3. Else if Type(***thisArg***) is not Object, set the `ThisBinding` to ToObject(***thisArg***).
```javascript
function func() {
   console.log(typeof this);
   console.log(this.length);
}
var primitive = "yumeijie";
var newFunc = func.bind(primitive);
newFunc();
// log "object" and "8"
```
4. Else set the `ThisBinding` to ***thisArg***.
> \#1 Setting the ThisBinding component of the current running execution context. And this is how the `this` is bound.

5. Let ***localEnv*** be the result of calling NewDeclarativeEnvironment passing the value of the **[[Scope]]** internal property of F as the argument.
6. Set the `LexicalEnvironment` to ***localEnv***.
7. Set the `VariableEnvironment` to ***localEnv***.
>  \#2 Setting the LexicalEnvironment  and VariableEnvironment components.

8. Let ***code*** be the value of F’s ***[[Code]]*** internal property.
9. Perform Declaration Binding Instantiation using the function code ***code*** and ***argumentList***.
>  \#3 Declaration Binding Instantiation.

## Summary

### 1. strict mode:

Take what the `thisArg` is:
1. ThisBinding <- thisArg

### 2. non-strict mode

Do something on `thisArg` according to it's type:
1. ThisBinding <- global object (***null*** or ***undefined***)
2. ThisBinding <- ToObject(thisArg) (***primitives*** except null and undefined)
> If thisArg is not valid object, try some convertions
3. ThisBinding <- thisArg (***object***)
