## Statements

```Syntax```

Statement :
- ***Block***   :star:
-  ***VariableStatement***  :star:
-  ***EmptyStatement***  :star: 
-  ***ExpressionStatement***  :star:
-  ***IfStatement***
- ***IterationStatement***
- ***ContinueStatement***
- ***BreakStatement***
- ***ReturnStatement***
- ***WithStatement***
- ***LabelledStatement***
- ***SwitchStatement***
- ***ThrowStatement***
- ***TryStatement***
- ***DebuggerStatement***


## Program

```Syntax```

***Program***:
- ***SourceElements***<sub>opt</sub>

***SourceElements***:
- ***SourceElement***
- ***SourceElements*** ***SourceElement***

***SourceElement***:
- ***Statement***
- ***FunctionDeclaration***

```Semantics```

The production ***Program*** :***SourceElements***<sub>opt</sub> is evaluated as follows:
1. The code of this ***Program*** is `strict mode code` if the Directive Prologue of its ***SourceElements*** contains a `Use Strict Directive` . If the code of this ***Program*** is `strict mode code`, ***SourceElements*** is evaluated in the following steps as `strict mode code`. Otherwise ***SourceElements*** is evaluated in the following steps as `non-strict mode code`.
> Step 1: decide the `mode` in which the ***SourceElements*** is evaluated.

2. If ***SourceElements*** is not present, return (normal, empty, empty).
> Return a ***Completion Specification Type***.

3. Let ***progCxt*** be a new **execution context** for ***global code***.
> Step 3: create and initialize a new ***execution context*** for global code.

4. Let ***result*** be the result of evaluating ***SourceElements***.
5. **Exit** the execution context ***progCxt***. :star: :star: :star: :star: :star:
> The ***proCxt*** will exit, but the ***Lexical Environment*** of the execution context may still retain in the memory, for it may be referenced by some function. When the ***proCtx*** exits the **execution context stack** is `empty`.
>
> But When some ***events*** happen like ***click events***, corresponding ***callbacks*** will be called and thus ***new execution contexts*** will be pushed into the **execution context stack** and when the ***callbacks*** finish and exit, the **execution context stack** again becomes `empty`. 

6. Return ***result***. :star:
> The ***global code***, too, returns a result.

####  Entering Global Code

The following steps are performed when control enters the execution context for ***global code***:
1. Initialize the ***execution context*** using the global code.
    1. Set the ***VariableEnvironment*** to the ***Global Environment***.
    2. Set the ***LexicalEnvironment*** to the ***Global Environment***.
    3. Set the ***ThisBinding*** to the ***global object***.
2. Perform ***Declaration Binding Instantiation*** using the ***global code***.

#### The Global Environment

The ***global environment*** is a `unique` **Lexical Environment** which is created before any ECMAScript code is executed. The global environment’s ***Environment Record*** is an ***object environment record*** whose ***binding object*** is the **global object**. The global environment’s ***outer environment reference*** is `null`.
