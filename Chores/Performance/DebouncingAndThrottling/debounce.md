# Debounce

The Debounce technique allow us to "group" multiple sequential calls in a single one.

![](https://github.com/Gyumeijie/assets/blob/master/web-front-learning/debounce.png)

The follow snippet is a very simple implementation:

```javascript
var debounce = function(func, wait) {
    var timeout;
    
    return function() {
      // If the function is used as an event handle, then the `this` keywork refers to 
      // the DOM Element to which the event handle is attacted
      var context = this, args = arguments;
      
      var later = function() {
        timeout = null;
        func.apply(context, args);
      };
      
      clearTimeout(timeout);
      timeout = setTimeout(later, wait);
    };
  };
```

Moreover, a cancellable version is like the following:

```javascript
var debounce = function(func, wait) {
    var timeout;
    
    var debounced = function() {
      // If the function is used as an event handle, then the `this` keywork refers to 
      // the DOM Element to which the event handle is attacted
      var context = this, args = arguments;
      
      var later = function() {
        timeout = null;
        func.apply(context, args);
      };
      
      clearTimeout(timeout);
      timeout = setTimeout(later, wait);
    };
    
    debounced.cancel = function() {
      clearTimeout(timeout);
      timeout = null;
    };

    return debounced;
  };

```
If you need to cancel a scheduled debounce, you can call .cancel() on the debounced function.


Imagine you are in an elevator. The doors begin to close, and suddenly another person tries to get on. The elevator doesn't begin its function to change floors, the doors open again. Now it happens again with another person. The elevator is delaying its function (moving floors), but optimizing its resources.

![](https://github.com/Gyumeijie/assets/blob/master/web-front-learning/debounce_tailing.webp)

## Leading edge(or "immediate")

The debounced version waits before triggering the function execution, until the events stop happening so ***rapidly***. Why not trigger the function execution immediately, so it behaves exactly as the original non-debounced handler? But not fire again until there is a pause in the ***rapid calls***.

![](https://github.com/Gyumeijie/assets/blob/master/web-front-learning/debounce_leading.webp)

```javascript
var debounce = function(func, wait, immediate) {
    var timeout, callNow = true;

    var debounced = function(args) {      
      var context = this, args = arguments;
      
      var later = function() {
          timeout = null;
          func.apply(context, args);
      };
      var signal = function () {
          callNow = true
      }
 
      clearTimeout(timeout);
      if (immediate) {     
          // wait for the signal of re-executing our function, immediately
          timeout = setTimeout(signal, wait); 
          if (callNow) {
              func.apply(this, args);
              callNow = false
          }
      } else {
        // wait for execution of our function
        timeout = setTimeout(later, wait); 
      }
    };

    debounced.cancel = function() {
      clearTimeout(timeout);
      timeout = null;
    };

    return debounced;
  };

```

# References

- [Debouncing and Throttling Explained Through Examples](https://css-tricks.com/debouncing-throttling-explained-examples/)
- [Debounce Your Input Handlers](https://developers.google.com/web/fundamentals/performance/rendering/debounce-your-input-handlers)
- [The Difference Between Throttling and Debouncing](https://css-tricks.com/the-difference-between-throttling-and-debouncing/) 
