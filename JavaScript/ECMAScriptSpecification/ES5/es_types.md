# ECMAScript Types

:star: In ECMAScript language, ***types*** are further subclassified into **ECMAScript language types** and **specification types**.

An **ECMAScript language type** corresponds to values that are directly manipulated by an ECMAScript programmer using the 
ECMAScript language. The ECMAScript language types are ***Undefined***, ***Null***, ***Boolean***, ***String***, ***Number***,
***Symbol***(added in ES6) and ***Object***.

A **specification type** corresponds to ***meta-values*** that are used within algorithms to describe the semantics of ECMAScript
language constructs and ECMAScript language types.  The specification types are ***Reference***, ***List***, ***Completion***, 
***Property Descriptor***, ***Property Identifier***, ***Lexical Environment***, and ***Environment Record***.

Within ECMAScript specification,:star: the notation ***“Type(x)”*** is used as shorthand for ***“the type of x”*** where ***“type”***
refers to the **ECMAScript language** and **specification types** defined above.


## The Completion Specification Type

The **Completion type** is used to explain the ***behaviour of statements*** (break, continue, return and throw) that perform 
***nonlocal*** transfers of control. 
> Does the "nonlocal" mean not in sequence?

***Values*** of the Completion type are triples of the form (***type***, ***value***, ***target***), where `type` is one of 
**normal**, **break**, **continue**, **return**, or **throw**, `value` is any ECMAScript language value or empty, and `target`
is any ECMAScript identifier or empty.
> What are ***value*** and ***target*** for?

:star: The term ***“abrupt completion”*** refers to any completion with a `type` other than **normal**.
