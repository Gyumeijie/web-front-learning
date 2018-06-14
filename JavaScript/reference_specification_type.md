## Reference Specification Type

***References*** are only a **mechanism**, [used to describe certain behaviors in ECMAScript](https://es5.github.io/#x8.7).
They're not really "visible" to the outside world. They are vital for engine implementors, and users of the language don't need to know about them.

## Theory

ECMAScript defines ***Reference*** as a ***"resolved name binding".*** It's an abstract entity that consists of three components
— ***base***, ***name***, and ***strict flag***. 

There are two cases when ***Reference*** is **created**: in the process of ***Identifier resolution*** and during ***property access***.
> It is important to know whena **Reference** is created.

Every time a ***Reference*** is created, its components — ***"base"*** , ***"name"*** , ***"strict"*** — are set to some values. 
The strict flag is easy — it's there to denote if code is in strict mode or not. The "name" component is set to identifier or 
property name that's being resolved, and the base is set to either property object or environment record.
> Note: the **base** value is either ***undefined***, an ***Object***, a ***Boolean***, a ***String***, a ***Number***, or 
an ***environment record***.  A base value of ***undefined*** indicates that the reference could not be resolved to a binding. 
The referenced name is a String. 

It might help to think of References as ***plain JS objects with a null [[Prototype]]*** (i.e. with no "prototype chain"), 
containing only "base", "name", and "strict" properties; this is how we can illustrate them below:

When Identifier foo is ***resolved***, a **Reference** is created like so:
```javascript
// when foo is defined earlier

foo; // Indentifier Resolution for 'foo' happens here

// creates (in abstract code)
var Reference = {
  base: EnvironmentRecord,
  name: "foo",
  strict: false
};
```

#### #1 Identifier Resolution

***Identifier resolution*** is the process of determining the ***binding*** of an ***Identifier*** using the ***LexicalEnvironment*** 
of the ***running execution context***. 

During ***execution*** of ECMAScript code, the syntactic production PrimaryExpression : Identifier is evaluated using the following algorithm:
1. Let ***env*** be the running execution context’s ***LexicalEnvironment***.
2. If the syntactic production that is being evaluated is contained in a ***strict mode code***, then let strict be **true**, else let strict be **false**.
3. Return the result of calling ***GetIdentifierReference*** function passing ***env***, ***Identifier***, and ***strict*** as arguments. 
(**return a value of type Reference**)

> The result of evaluating an identifier is always a value of type Reference with its referenced name component equal to the Identifier String.

The abstract operation ***GetIdentifierReference*** is called with a Lexical Environment ***lex***, an ***identifier*** String
name, and a Boolean flag ***strict***. The value of ***lex*** may be **null**. When called, the following steps are performed:
(***A simple lookup through the chain of lexical environments***)

1. If ***lex*** is the value **null**, then
    1. Return a value of type **Reference** whose base value is **undefined**, whose referenced name is name, and whose strict
  mode flag is strict.
> The identifier cann't not find in any Lexical Environment.

2. Let ***envRec*** be ***lex***’s environment record.
3. Let ***exists*** be the result of calling the HasBinding(N) concrete method of ***envRec*** passing ***name*** as the argument N.
4. If ***exists*** is **true**, then
    1. Return a value of type **Reference** whose base value is **envRec**, whose referenced name is name, and whose strict mode flag is strict.
> Note: the ***envRec*** can be either a ***object environment record*** or ***declarative environment record***.

5. Else
   1. Let ***outer*** be the value of ***lex***’s outer environment reference.
   1. Return the result of calling **GetIdentifierReference** passing ***outer***, ***name***, and ***strict*** as arguments.
 > Recursively call the GetIdentifierReference.

#### section(#1) summary 
The ***base*** value of a ***Reference*** created in the process of  **Identifier resolution** can be a ***environment record*** or ***undefined***.

----------------

```javascript
foo.bar;

// creates (in abstract code)
var Reference = {
  base: foo,
  name: "bar",
  strict: false
};
```

####  #2 Property Accessor

***Properties*** are accessed by name, using either the ***dot notation***:
```
MemberExpression.IdentifierName
CallExpression.IdentifierName
```
or the ***bracket notation***:
```
MemberExpression[Expression] 
CallExpression[Expression]
```

The production MemberExpression : MemberExpression [ Expression ] is evaluated as follows:

1. Let ***baseReference*** be the result of evaluating ***MemberExpression***.
2. Let ***baseValue*** be GetValue(***baseReference***).
> For  example `(123)["toString"]` , then the ***base*** value equals to ***Number***?

3. Let ***propertyNameReference*** be the result of evaluating ***Expression***.
4. Let ***propertyNameValue*** be GetValue(***propertyNameReference***).
5. Call CheckObjectCoercible(baseValue). 
6. Let ***propertyNameString*** be ToString(***propertyNameValue***).
7. If the syntactic production that is being evaluated is contained in strict mode code, let strict be true, else let strict be false.
8. Return a value of type **Reference** whose base value is ***baseValue*** and whose referenced name is ***propertyNameString***, 
and whose strict mode flag is strict.


The abstract operation ***CheckObjectCoercible*** throws an **error** if its argument is a value that cannot be converted to an
Object using ToObject. 

Argument Type | Result
----  |  ---
Undefined | Throw a TypeError exception.
Null | Throw a TypeError exception.
Boolean | Return 
Number | Return 
String | Return 
Object | Return 

```javascript
(123).toString            (123)["toString"]
(true).toString           (true)["toString"]
("string").toString       ("string")["toString"]
```

#### section(#2) summary 
The ***base*** value of a ***Reference*** created during **Property Accessing** can be a ***Number*** , ***String***, ***Boolean*** 
or an ***Object***.

----------------

Essentially, ***References*** are a simple mechanism of representing name bindings; it's a way to abstract both object-property
resolution and variable resolution into a ***unified data structure*** — base + name — whether that base is a regular JS object
(as in property access) or an Environment Record (as in identifier resolution).


```javascript
name = "global";
obj = {
   name: "obj",
   getName: function() {
      alert(this.name);
   }
}

with(obj) {
    getName();
}
```

As shown above, the reolution of the ***getName*** in the with statement will create a Reference, which has the following settings:
```javascript
var ref = {
  base: EnvironmentRecord,
  name: "getName",
  strict: false
};

```
The value of ***base***  is further an ***Object Environment Record*** which acts as the ***LexcalEnvironment*** of the current 
running execution context. But in order to explain the result of calling the function ***getName***, we need have some more knowleadge
about the ***Environment Record*** as shown below. 

- For **Declarative Environment Records**  The ***ImplicitThisValue***  always return ***undefined*** as their ImplicitThisValue. 
**Object Environment Records** return ***undefined*** as their ImplicitThisValue unless their ***provideThis*** flag is **true**.  

- ***Object Environment Records*** can be configured to provide their ***binding object*** as an ***implicit this value*** for
use in ***function calls***. This capability is used to specify the behaviour of **With Statement** induced bindings. The capability 
is controlled by a **provideThis** Boolean value that is associated with each object environment record. By default, the value of 
**provideThis** is **false** for any object environment record.

- When calling a function, if the ***base*** of the result of evaluating ***MemberExpression*** is an ***Environment Record***,  
***thisValue*** is set to the result of calling the ***ImplicitThisValue*** concrete method of GetBase(ref), where ***ref*** is
the result of evaluating ***MemberExpression*** .
> The function calls take the form of [CallExpression : MemberExpression Arguments](https://es5.github.io/#x11.2.3)

So when the funciton ***getName*** is called, the ***thisValue*** is set to  the result of calling the ***ImplicitThisValue*** 
concrete method of GetBase(ref), the binding object ***obj*** of the ***Object Environment Record***.  So when the function code
is executed, it will alert "obj" instead of "global".

