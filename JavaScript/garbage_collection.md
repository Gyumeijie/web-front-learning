# Introduction

Low-level languages, like C, have low-level memory management primitives like malloc() and free(). On the other hand, 
JavaScript values are allocated when things (objects, strings, etc.) are created and "automatically" freed when they are
not used anymore. The latter process is called garbage collection. This "automatically" is a source of confusion and gives
JavaScript (and other high-level languages) developers the impression they can decide not to care about memory management. 
**This is a mistake**.

# Memory life cycle

Regardless of the programming language, memory life cycle is pretty much always the same:

1. Allocate the memory you need
2. Use the allocated memory (read, write)
3. Release the allocated memory when it is not needed anymore

The second part is explicit in all languages. The first and last parts are explicit in low-level languages but are mostly 
implicit in high-level languages like JavaScript.

### #1 Allocation in JavaScript

- Value initialization
```javascript
var o = {
  a: 1,
  b: null
}; // allocates memory for an object and contained values
```
- Allocation via function calls
```javascript
var d = new Date(); // allocates a Date object
var e = document.createElement('div'); // allocates a DOM element

```

### #2 Using values

Using values basically means reading and writing in allocated memory. This can be done by reading or writing the value of 
a variable or an object property or even passing an argument to a function.


### #3 Release when the memory is not needed anymore

The hardest task here is to find when **"the allocated memory is not needed any longer"**. It often requires the developer to 
determine where in the program such piece of memory is not needed anymore and free it.

High-level languages embed a piece of software called **"garbage collector"** whose job is to track memory allocation and use in 
order to find when a piece of allocated memory is not needed any longer in which case, it will automatically free it. 
This process is an approximation since the general problem of knowing whether some piece of memory is needed is **undecidable** 
(can't be solved by an algorithm).

#  Garbage collection

The general problem of automatically finding whether some memory "is not needed anymore" is **undecidable**. As a consequence, 
garbage collections implement a **restriction** of a solution to the general problem.

## Reachability

The main concept of **memory management** in JavaScript is **reachability** or **reference**.

Simply put, "reachable" values are those that are accessible or usable somehow. They are guaranteed to be stored in memory.

1. There's a base set of inherently reachable values, that cannot be deleted for obvious reasons.

    For instance:
    
    - Local variables and parameters of the current function.
    - Variables and parameters for other functions on the current chain of nested calls.
    - Global variables.
    - (there are some other, internal ones as well)

    These values are called *roots*.

2. Any other value is considered reachable if it's reachable from a root by a reference or by a chain of references.

    For instance, if there's an object in a local variable, and that object has a property referencing another object, that 
    object is considered reachable. And those that it references are also reachable.

There's a background process in the JavaScript engine that is called [garbage collector](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)). 
It monitors all objects and removes those that have become unreachable.

> The notion of "object" is extended to something broader than **regular JavaScript objects** and also contains 
**function scopes** (or the **global lexical scope**).

## Reference-counting garbage collection

This is the most naive garbage collection algorithm. This algorithm reduces the definition of "an object is not needed anymore"
to **"an object has no other object referencing to it"**. An object is considered garbage collectible if there is zero reference 
pointing at this object.

> Limitation: 
the reference-counting algorithm considers that since each of the two objects is referenced at least once, thus creating a cycle, 
neither can be garbage-collected.

## Mark-and-sweep algorithm

This algorithm reduces the definition of "an object is not needed anymore" to **"an object is unreachable"**.

This algorithm assumes the knowledge of a set of objects called roots (In JavaScript, the root is the global object). 
Periodically, the garbage-collector will start from these roots, find all objects that are referenced from these roots, 
then all objects referenced from these, etc. Starting from the roots, the garbage collector will thus find all reachable 
objects and collect all non-reachable objects.

All improvements made in the field of JavaScript garbage collection (generational/incremental/concurrent/parallel garbage collection) 
over the last few years are implementation improvements of this algorithm, but not improvements over the garbage collection algorithm 
itself nor its reduction of the definition of when "an object is not needed anymore". 

They are just some **optimizations** which JavaScript engines apply to make it run faster and not affect the execution.

- **Generational collection** -- objects are split into two sets: "new ones" and "old ones". Many  objects appear, do their job
and die fast, they can be cleaned up aggressively. Those that survive for long enough, become "old" and are examined less often.

> different check frequency on different types of objects.

- **Incremental collection** -- if there are many objects, and we try to walk and mark the whole object set at once, it may take
some time and introduce visible delays in the execution. So the engine tries to split the garbage collection into pieces. Then 
the pieces are executed one by one, separately. That requires some extra bookkeeping between them to track changes, but we have 
many tiny delays instead of a big one.

> split the garbage collection into pieces.

- **Idle-time collection** -- the garbage collector tries to run only while the CPU is idle, to reduce the possible effect on
the execution.




