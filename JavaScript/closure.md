# What is a Closure?
A ***closure*** is the combination of a **function** and the **lexical environment** within which that function was declared.

In JavaScript, ***closures*** are created every time a function is created, at function creation time. For when a function is created, a new function object is created and it's internal ***[[Scope]]*** in ES5 or ***[[Environment]]*** in ES6 keeps the lexical environment of the current running execution context.

Version | Internal Property(Slot) | Value Type | Description
-- | -- | -- | --| 
ES5 |[[Scope]] | Lexical Environment | A lexical environment that defines the environment in which a Function object is executed. Of the standard built-in ECMAScript objects, only Function objects implement [[Scope]].
ES6 | [[Environment]] | Lexical Environment | The Lexical Environment that the function was closed over. Used as the outer environment when evaluating the code of the function.

---

### Block Evaluation

In ES6, when control enters a block, the following steps are taken:
1. Let ***oldEnv*** be the running execution context's ***LexicalEnvironment***.
2. Let ***blockEnv*** be NewDeclarativeEnvironment(***oldEnv***).
3. Perform **BlockDeclarationInstantiation**(***StatementList***, ***blockEnv***).
4. Set the running execution context's LexicalEnvironment to ***blockEnv***.
> Now the LexicalEnvironment of  running execution context is the new ***blockEnv***.

5. Let ***blockValue*** be the result of evaluating ***StatementList***.
6. Set the running execution context's LexicalEnvironment to ***oldEnv***.
7. Return ***blockValue***.

> Note that no matter how control leaves the Block the LexicalEnvironment is always restored to its former state

### BlockDeclarationInstantiation( code, env )

When a Block or CaseBlock is evaluated a new declarative Environment Record is created and bindings for each block scoped variable, constant, function, or class declared in the block are instantiated in the Environment Record. And  This Environment Record forms a so-called **Block Scope**.

BlockDeclarationInstantiation is performed as follows using arguments code and env. ***code*** is the Parse Node corresponding to the body of the block. ***env*** is the Lexical Environment in which bindings are to be created.

1. Let ***envRec*** be ***env***'s EnvironmentRecord.
2. Assert: ***envRec*** is a declarative Environment Record.
3. Let ***declarations*** be the ***LexicallyScopedDeclarations*** of code.
4. For each element ***d*** in declarations, do
    1. For each element ***dn*** of the BoundNames of ***d***, do
        1. If IsConstantDeclaration of ***d*** is true, then
            1. Perform ***envRec***.CreateImmutableBinding(***dn***, true).
        2. Else,
            1. Perform ***envRec***.CreateMutableBinding(***dn***, false).
    2. If ***d*** is a FunctionDeclaration, a GeneratorDeclaration, or an AsyncFunctionDeclaration, then
       1. Let ***fn*** be the sole element of the BoundNames of ***d***.
       2. Let ***fo*** be the result of performing InstantiateFunctionObject for ***d*** with argument ***env***.
       3. Perform ***envRec***.InitializeBinding(***fn***, ***fo***).

> LexicallyScopedDeclarations like ***let*** or ***const*** declarations are created in ***Block Scope***.
> 
> ***FunctionDeclarations***, ***GeneratorDeclarations***, and ***AsyncFunctionDeclaration*** in a block are only declared within 
the scope of the block and do not exist in the global scope through ***hoisting***.


Variable declarations with ***var*** are hoisted and not declared within the ***block scope***, for example:

```javascript
function testBlock(cond){
   if (cond) {
      var action = "can be accessible outside the block";
    } 
   console.log(action);
}
testBlock(true);
// "can be accessible outside the block";
```

In strict mode of ***ES5***, ***function declarations*** cannot be nested inside block. In non-strict
mode, the results were unpredictable. Different browsers and engines implemented their own rules for how they would handle 
function declarations inside blocks.

ECMAScript 2015(***ES6***) to the extent that ***function declarations*** are now allowed ***inside blocks***. In an ES2015 environment,
a function declaration inside of a block will be scoped inside that block. 







