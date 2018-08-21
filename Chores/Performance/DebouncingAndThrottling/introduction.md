# Introduction

***Debounce*** and ***throttle*** are two similar techniques to control how many times we allow a function to be executed over time.

Having a debounced or throttled version of our function is especially useful when we are attaching the function to a ***DOM event***. Because we are giving ourselves a layer of control between the event and the execution of the function; In some cases where a event handle doing a lot computing work would be trigged frequently, then the execution of the handle may block UI rendering heavily. 

Remember, we don't, and actually are unable to, control how often those DOM events are going to be emitted. what we can control is the frequency of our original function being called but not the debounced or throttled version one. And a debounced or throttled version will be called every time when a DOM event was emitted. :star:

![](https://github.com/Gyumeijie/assets/blob/master/web-front-learning/debounced_or_throttled_version.png)

***requestAnimationFrame*** is another way of **rate-limiting** the execution of a function.It can be thought as a ```throttle(func, 16)```. But with a much ***higher fidelity***, since it's a browser native API that aims for **better accuracy**. We can use the ***rAF API***, as an alternative to the ***throttle*** function. 
