## Hoisting

The hoisting is acutally the process of `Declaration Binding Instantiation` which is performed when control enters an execution context.

## Declaration Binding Instantiation

Every ***execution context*** has an associated VariableEnvironment. ***Variables*** and ***functions*** declared in ECMAScript code 
evaluated in an execution context are added as bindings in that VariableEnvironment’s Environment Record. For function code, 
***parameters*** are also added as bindings to that Environment Record.

Which Environment Record(***Object Environment Record*** and ***Declarative Environment Record***) is used to bind a declaration 
and its kind depends upon the type of ECMAScript code executed by the execution context, but the remainder of the behavior is 
generic. On entering an execution context, bindings are created in the VariableEnvironment as follows using the caller provided 
code and, if it is function code, argument List ***args***:
> When to use which Environment Record?

1. Let ***env*** be the ***environment record*** component of the ***running execution context***’s ***VariableEnvironment***.
2. If code is ***eval code***, then let configurableBindings be true else let configurableBindings be false.
3. If code is ***strict mode code***, then let ***strict*** be true else let ***strict*** be false.
4. If code is function code, then
   1. Let ***func*** be the function whose [[Call]] internal method initiated execution of code. Let ***names*** be the value 
   of func’s [[FormalParameters]] internal property.
   > ***names*** are formal parameters, ***a list of names***.
   2. Let ***argCount*** be the number of elements in ***args***.
   > ***args*** are arguments passed in, ***a list of values***.
   3. Let ***n*** be the number 0.
   4. For each String ***argName*** in ***names***, in list order do
      1. Let ***n*** be the current value of n plus 1.
      2. If ***n*** is greater than ***argCount***, let ***v*** be undefined otherwise let ***v*** be the value of the ***n’th*** element of ***args***.
      3. Let ***argAlreadyDeclared*** be the result of calling ***env***’s HasBinding concrete method passing ***argName*** as the argument.
      4. If ***argAlreadyDeclared*** is false, call ***env***’s CreateMutableBinding concrete method passing ***argName*** as the argument.
      5. Call ***env***’s SetMutableBinding concrete method passing ***argName***, ***v***, and ***strict*** as the arguments.

> Step 4: create ***names*** in ***env*** and binding the ***args***(value) to ***the formal parameters***(names).

---------

5.For each FunctionDeclaration ***f*** in ***code***, **in source text order** do    
   1. Let ***fn*** be the ***Identifier*** in FunctionDeclaration ***f***.
   > FunctionDeclaration is different from FuctionExpression
   2. Let ***fo*** be the result of **instantiating** FunctionDeclaration ***f***.
   > ***fo*** then refers to the newly-created funciton object
   3. Let ***funcAlreadyDeclared*** be the result of calling ***env***’s HasBinding concrete method passing ***fn*** as the argument.
   4. If ***funcAlreadyDeclared*** is **false**, call ***env***’s SetMutableBinding concrete method passing ***fn***, ***fo***, and ***strict*** as the arguments. (**simplified**)

> Step 5: processing ***FunctionDeclarations***.  

---------

6. Let ***argumentsAlreadyDeclared*** be the result of calling ***env***’s HasBinding concrete method passing ***"arguments"*** as the argument.
7. If ***code*** is function code and ***argumentsAlreadyDeclared*** is **false**, then  
   1. Let ***argsObj*** be the result of calling the abstract operation **CreateArgumentsObject** passing ***func***, ***names***, ***args***, ***env*** and ***strict*** as arguments.
   2. If ***strict*** is true, then
      1. Call ***env***’s **CreateImmutableBinding** concrete method passing the String ***"arguments"*** as the argument.
      2. Call ***env***’s **InitializeImmutableBinding** concrete method passing ***"arguments"*** and ***argsObj*** as arguments.
   3. Else,
      1. Call ***env***’s **CreateMutableBinding** concrete method passing the String ***"arguments"*** as the argument.
      2. Call ***env***’s **InitializeImmutableBinding** concrete method passing ***"arguments"***, ***argsObj*** and ***false*** as arguments.

> Steps 6 and 7: Create ***arguments object*** if it is not declared.  

In the following example, there is no ***argument object*** is created.
```javascript
function useArguments(arg1){
   var arguments = arg1;
}
```
---------

8. For each ***VariableDeclaration*** and ***VariableDeclarationNoIn*** d in code, **in source text order** do
   1. Let ***dn*** be the Identifier in ***d***.
   2. Let ***varAlreadyDeclared*** be the result of calling ***env***’s HasBinding concrete method passing ***dn*** as the argument.
   3. If ***varAlreadyDeclared*** is **false**, then
      1. Call ***env***’s **CreateMutableBinding** concrete method passing ***dn*** and ***configurableBindings*** as the arguments.
      2. Call ***env***’s **SetMutableBinding** concrete method passing ***dn***, ***undefined***, and ***strict*** as the arguments.
      > variables are initialized with value ***undefined***, and functions are initialize with corresponding ***function object***.

> Step 8: processing ***VariableDeclarations***

---------

Note that ***FunctionDeclarations*** are processed before ***VariableDeclarations***, so If a variable has the same name with
a function, then **name** will be bound to the ***function object***(FunctionDeclarations) not the ***undefined***(VariableDeclarations)

```javascript
console.log(sameName)
var sameName = "I am a variable"
function sameName(){
  console.log("I am a function!")
}
console.log(sameName)
```
The `console.log()` outputs: 
```javascript
ƒ sameName(){
  console.log("I am a function!")
}
```
and the second outputs: 
```javacript
I am a variable
```

> Warning: a variable with an ***Initialiser*** is assigned the value of its ***AssignmentExpression*** when the VariableStatement is
**executed**, not when the variable is **~~created~~**(during the process of `Declaration Binding instantiation`).

