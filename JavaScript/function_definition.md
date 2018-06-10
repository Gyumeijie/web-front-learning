# Lexical Environments
A Lexical Environment is a specification type used to define the **association of Identifiers to specific variables and functions** based upon the lexical nesting structure of ECMAScript code.

A Lexical Environment consists of an Environment Record and a possibly null reference to an outer Lexical Environment.

```Lexical Environment``` |
---|
Environment Record |
outer Lexical Environment |
> The struct of Lexical Environment likes single-linked list: the ***Environment Record*** is the ***data*** and the ***outer Lexical Environment*** is the ***link***.
>
> The Lexical Environment is used to  ***resolve identifier references*** made by code within this execution context.

An Environment Record records the identifier bindings that are created within the scope of its associated Lexical Environment. There are two kinds of Environment Record values used in this specification: 
1. ***declarative environment records***.
>Declarative environment records are used to define the effect of ECMAScript language syntactic elements such as ***FunctionDeclarations***, ***VariableDeclarations***, and ***Catch clauses*** that directly associate ```identifier bindings``` with ECMAScript language values. 

2. ***object environment records***.
> Object environment records are used to define the effect of ECMAScript elements such as ***Program*** and ***WithStatement*** that associate ```identifier bindings``` with the properties of some object.

The outer environment reference is used to model the logical nesting of Lexical Environment values. 

A Lexical Environment may serve as the outer environment for multiple inner Lexical Environments. For example, if a FunctionDeclaration contains two nested FunctionDeclarations then the Lexical Environments of each of the nested functions will have as their outer Lexical Environment the Lexical Environment of the current execution of the surrounding function.

#### Abstract methods on the Environment Records 
- HasBinding
- CreateMutableBinding
- SetMutableBinding
- GetBindingValue
- DeleteBinding
- ImplicitThisValue

#### Operations of the Lexical Environment
- GetIdentifierReference (lex, name, strict)

The abstract operation GetIdentifierReference is called with a Lexical Environment ***lex***, an identifier String ***name***, and a Boolean flag ***strict***. The value of lex may be null. When called, the following steps are performed(**recursively**):
1. If ***lex*** is the value null, then
a. Return a value of type Reference whose base value is undefined, whose referenced name is name, and whose strict mode flag is strict.Let envRec be lex’s environment record.
> If ***lex*** is null, return an undefined Reference.

2. Let ***envRec*** be ***lex***’s environment record.
3. Let ***exists*** be the result of calling the HasBinding(N) concrete method of envRec passing name as the argument N.
4. If ***exists*** is true, then
   1. Return a value of type Reference whose base value is ***envRec***, whose referenced name is ***name***, and whose strict mode flag is ***strict***.
>  The lex's environment record has a binding named ***name***, then return it.

5. Else
   1. Let ***outer*** be the value of lex’s outer environment reference.
   2. Return the result of calling GetIdentifierReference passing ***outer***, ***name***, and ***strict*** as arguments.
> If the current environment record doesn't contain the binding go on to the lex’s ***outer environment***.


- NewDeclarativeEnvironment (E)

When the abstract operation NewDeclarativeEnvironment is called with either a Lexical Environment or null as argument ***E*** the following steps are performed:
1. Let ***env*** be a new Lexical Environment.
> Create a empty new Lexical Environment.

2. Let ***envRec*** be a new declarative environment record containing no bindings.
3. Set ***env’s*** environment record to be ***envRec***.
4. Set the outer lexical environment reference of ***env*** to ***E***.
> Initialize the ***environment record*** and the ***outer lexical environment reference***.

5. Return ***env***.
> Return  the new initialized Lexical Environment.

- NewObjectEnvironment(O, E)

When the abstract operation NewObjectEnvironmentis called with an Object ***O*** and a Lexical Environment ***E*** (or null) as arguments, the following steps are performed:
1. Let ***env*** be a new Lexical Environment.
> Create a empty new Lexical Environment.

2. Let ***envRec*** be a new object environment record containing ***O*** as the binding object.

3. Set ***env***’s environment record to be ***envRec***.
4. Set the outer lexical environment reference of env to ***E***.
> Initialize the ***environment record*** and the ***outer lexical environment reference***.

5. Return env.
 > Return  the new initialized Lexical Environment

## summary
- The Lexical Environment is used to resolve identifier references.
- The Lexical Environment and it's environment record all have a set of methods.

# Function Definition
The production 
***FunctionDeclaration***: **function** Identifier (***FormalParameterList***<sub>opt</sub>) { ***FunctionBody*** } 
is **instantiated** as follows during **Declaration Binding instantiation**

1. Return the result of creating a new Function object and pass in:
 - parameters specified by ***FormalParameterList***<sub>opt</sub>.
 - body specified by ***FunctionBody***.
 - VariableEnvironment of the running execution context as the ***Scope***. 
 - true as the ***Strict flag*** if the FunctionDeclaration is contained in strict code or if its FunctionBody is strict code.

The production
***FunctionExpression*** : **function** (***FormalParameterList***<sub>opt</sub>) { ***FunctionBody*** } 
is **evaluated** as follows:

1. Return the result of creating a new Function object and pass in:
 - parameters specified by ***FormalParameterList***<sub>opt</sub>.
 - body specified by ***FunctionBody***.
 - LexicalEnvironment of the running execution context as the ***Scope***. 
 - true as the ***Strict flag*** if the FunctionDeclaration is contained in strict code or if its FunctionBody is strict code.

The production
***FunctionExpression*** : **function** Identifier (***FormalParameterList***<sub>opt</sub>) { ***FunctionBody*** } 
is **evaluated** as follows:

1. Let ***funcEnv*** be the result of calling NewDeclarativeEnvironment passing the running execution context’s ***LexicalEnvironment*** as the argument.
2. Let ***envRec*** be ***funcEnv’s*** environment record.
3. Call the CreateImmutableBinding(N) concrete method of ***envRec*** passing the String value of ***Identifier*** as the argument.
> Create a new Lexical Environment for holding the **indentifier**.
>
> Note that since the identifier is not bound to the ***LexicalEnvironment*** of the running execution context, and the lookup of an identifier is always upward  the ***scope chain*** but not downward the chain, so it is cannot be referenced from and does not affect the enclosing scope.
>
> It can only be referenced from inside the FunctionExpression's FunctionBody to allow the function to call itself recursively. 

4. Let ***closure*** be the result of creating a new Function object:
 - parameters specified by ***FormalParameterList***<sub>opt</sub>.
 - body specified by ***FunctionBody***.
 - **funcEnv** as the ***Scope***. 
 - true as the ***Strict flag*** if the FunctionDeclaration is contained in strict code or if its FunctionBody is strict code.
 > Create a object function.

5. Call the InitializeImmutableBinding(N,V) concrete method of ***envRec*** passing the String value of ***Identifier*** and ***closure*** as the arguments.
> Set the ***Identifier*** to the new created function object ***closure***. 

6. Return closure.
> Return the function object.

## summary
- Instantiating a function declaration and evaluating a function expression all lead to create a function object.
- Evaluating a function expression with an identifier brings an extra Lexical Environment which only includes the identifier's binding.


# Creating Function Objects

Given an optional parameter lis specified by ***FormalParameterList***, a body specified by ***FunctionBody***, a Lexical Environment specified by ***Scope***, and a Boolean flag ***Strict***, a Function object is constructed as follows:

1. Create a new native ECMAScript object and let ***F*** be that object.
2. Set the internal methods.
3. Set ***[[Class]],*** ***[[Prototype]],***  ***[[Get]],*** ***[[Call]],*** ***[[Co nstruct]]*** and ***[[HasInstance]]*** internal properties.
4. Set the ***[[Scope]]*** internal property of ***F*** to the value of ***Scope***.
> Hold the reference to the Lexical Environment of the current running execution context or the Lexical Environment created by the function expression with ***identifier***.

5. Let ***names*** be a List containing, in left to right textual order, the Strings corresponding to the identifiers of ***FormalParameterList***.
6. Set the ***[[FormalParameters]]*** internal property of ***F*** to names.
> Store formal parameters.

7. Set the ***[[Code]]*** internal property of ***F*** to ***FunctionBody***.
> Store function body.

8. More settings ......

### summary 
- Creating a function object is almost just a list of settings.
