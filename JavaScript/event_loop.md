# JavaScript event loop explained

"How is JavaScript asynchronous and single-threaded?" The short answer is that JavaScript language is **single-threaded**
and the **asynchronous behaviour is not part of the JavaScript language itself**, rather they are built on top of the core 
JavaScript language in the browser (or the programming environment, say node) and accessed through the browser APIs.

> Note that Javascript is single-threaded, but the browser is not

# Basic architecture

![architecture](../assets/event_loop.png)

- ## heap
Objects are allocated in a heap which is just a name to denote a large mostly unstructured region of memory.

- ## stack
This represents the single thread provided for JavaScript code execution. Function calls form a stack of frames.

- ## Web APIs
They are built into your web browser, and are able to expose data from the browser and surrounding computer
environment and do useful complex things with it. They are not part of the JavaScript language itself, rather they are built 
on top of the core JavaScript language, providing you with extra superpowers to use in your JavaScript code. 
> Browser Web APIs implemented in C++ to handle async events like DOM events, http request, setTimeout, etc.

- ## task queue or callback queue
Any of the web APIs pushes the callback on to the task queue when it's done.

- ## event loop
The event loop's job is to look at the stack and look at the task queue. If the stack is empty it takes the first thing on the 
queue and pushes it on to the stack which effectively run it.

# Example

```javascript
setTimeout(function(){
   console.log("hello");
}, 0);

console.log("world");

for (let i=0; i<1000000000; i++);
```
when run the code snippet above, the console will first print `world` and then `hello`. For the ```console.log("world");``` is 
synchronous task and will be taken high priority to execute, the `for loop` is also a synchronous task. Only after all the synchronous
tasks finished, and the stack is empty, asynchronous tasks already in queue can be processed.
