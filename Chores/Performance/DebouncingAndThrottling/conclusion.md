# Conclusion

Use ```debounce```, ```throttle``` and ```requestAnimationFrame``` to optimize your ***event handlers***. Each technique is slightly different, but all three of them are useful and complement each other.

As a rule of thumb, The ```requestAnimationFrame``` can be used if your JavaScript function is "painting" or animating directly properties, use it at everything that involves re-calculating element positions.

To make Ajax requests, or deciding if adding/removing a class (that could trigger a CSS animation), ```debounce``` or ```throttle``` is preferred, where you can set up ***lower executing rates*** (200ms for example, instead of 16ms)


In summary:

- debounce: Grouping a ***sudden burst*** of events (like keystrokes) into a single one.
- throttle: Guaranteeing a ***constant flow*** of executions every X milliseconds. Like checking every 200ms your scroll position to trigger a CSS animation.
- requestAnimationFrame: a ***throttle alternative***. When your function recalculates and renders elements on screen and you want to guarantee smooth changes or animations. Note: no IE9 support.

