# Timers
The `setTimeout()` and `setInterval()` methods allow authors to schedule timer-based callbacks. `clearTimeout(handle)` and `clearInterval(handle)` methods cancel the timeout set with `setInterval()` or `setTimeout()` identified by handle. These four methods are defined in **global** object in node or **window** object in browser.

## setTimeout

- handle = setTimeout(handler [, timeout [, arguments...] ])

Schedules a timeout to run ***handler*** after ***timeout*** milliseconds. Any ***arguments*** are passed straight through to the ***handler***.

```javascript
function hello(name){
   console.log("hello ", name)
}
var timeout = setTimeout(hello, 1000, "yumeijie")
// hello yumeijie
```

- handle = setTimeout(code [, timeout]) 

Schedules a timeout to **compile and run** ***code*** after ***timeout*** milliseconds. :alien:
```javascript
setTimeout("console.log(`hello yumeijie`)", 1000)
```

## clearTimeout

- clearTimeout(handle)

Cancels the timeout set with `setTimeout()` or `setInterval()` identified by ***handle***. :confused:

```javascript
function hello(name){
   console.log("hello ", name)
}
timeout = setTimeout(hello, 1000, "yumeijie")
clearInterval(timeout)
```

```javascript
function hello(name){
   console.log("hello ", name)
}
timeout = setInterval(hello, 1000, "yumeijie")
clearTimeout(timeout)
```

## setInterval

- handle = setInterval(handler [, timeout [, arguments...] ])

Schedules a timeout to run ***handler*** every ***timeout*** milliseconds. Any ***arguments*** are passed straight through to the ***handler***.


- handle = setInterval(code [, timeout]) 

Schedules a timeout to **compile and run** ***code*** every ***timeout*** milliseconds. :alien:
```javascript
setInterval("console.log(`hello yumeijie`)", 1000)
```

## clearInterval

- clearInterval(handle)

Cancels the timeout set with `setInterval()` or `setTimeout()` identified by ***handle***. :confused:


> The timer-set APIs do not guarantee that timers will run exactly on schedule. Delays due to CPU load, other tasks, etc, are to be expected. :heavy_exclamation_mark:

# References
- [Timers](https://html.spec.whatwg.org/#timers)
