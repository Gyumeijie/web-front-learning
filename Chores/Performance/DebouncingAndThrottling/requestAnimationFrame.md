# requestAnimationFrame(rAF)

```requestAnimationFrame``` is another way of ***rate-limiting*** the execution of a function.

It can be thought as a ```throttle(dosomething, 16)```. But with a much higher fidelity, since it's a browser native API that aims for better accuracy.

We can use the ***rAF*** API, as an alternative to the throttle function, considering this pros/cons:

- pros
  - Aims for 60fps (frames of 16 ms) but internally will decide the best timing on how to schedule the rendering.
  - Fairly simple and standard API, not changing in the future. Less maintenance.
  
- Cons
  - The start/cancelation of rAFs it's our responsibility, unlike .debounce or .throttle, where it's managed internally.
  - rAF is not supported in node.js, so you can't use it on the server to throttle filesystem events
  - Although all modern browsers offer rAF, still is not supported in IE9, Opera Mini and old Android. A polyfill would be needed still today.
  
  
# References

- [Debouncing and Throttling Explained Through Examples](https://css-tricks.com/debouncing-throttling-explained-examples/)
- [Debounce Your Input Handlers](https://developers.google.com/web/fundamentals/performance/rendering/debounce-your-input-handlers)
