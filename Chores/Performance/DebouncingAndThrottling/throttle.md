# Throttle

By using ***throttle***, we don't allow to our function to execute more than once every X milliseconds.

The main difference between this and debouncing is that throttle guarantees the execution of the function ***regularly***, at least every X milliseconds.

![](https://github.com/Gyumeijie/assets/blob/master/web-front-learning/throttle.png)

The following snippet is a very simple implementation:

```javascript
var throttle = function(func, wait) {
    var timeout;
 
    return function() {
      var context = this, args = arguments;

      var throttler = function() {
        timeout = null;
        func.apply(context, args);
      };
      
     if (!timeout) timeout = setTimeout(throttler, wait);
    };
};
```

Moreover, a cancellable version can be the following:

```javascript
var throttle = function(func, wait) {
    var timeout;
 
    var throttled = function() {
      var context = this, args = arguments;

      var throttler = function() {
        timeout = null;
        func.apply(context, args);
      };
      
     if (!timeout) timeout = setTimeout(throttler, wait);
    };

   throttled.cancel = function() {
      clearTimeout(timeout);
      timeout = null;
   };

   return throttled; 
};

```

If you need to cancel a scheduled throttle, you can call ```.cancel()``` on the throttled function.



# References

- [Debouncing and Throttling Explained Through Examples](https://css-tricks.com/debouncing-throttling-explained-examples/)
- [Debounce Your Input Handlers](https://developers.google.com/web/fundamentals/performance/rendering/debounce-your-input-handlers)
- [The Difference Between Throttling and Debouncing](https://css-tricks.com/the-difference-between-throttling-and-debouncing/) 
