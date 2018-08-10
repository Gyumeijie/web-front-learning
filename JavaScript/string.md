![string](../assets/string.svg)

## Caveats

- The **match** function returns different type of result depending on whether there is a `g` flag in regular expression or not.
```javascript

   var string = "repeat  repeat repeat repeat"
   var regexp = /repeat/       // without `g` flag
   string.match(regexp)
   // ["repeat", index: 0, input: "repeat  repeat repeat repeat", groups: undefined]  // Object
   
   regexp = /repeat/g         // with `g` flag
   string.match(regexp)
   //  ["repeat", "repeat", "repeat", "repeat"]  // Array
```
## Usage

- indexOf 

The parameter of ***indexOf*** can be either a primitive or an object.

```javascript
var obj = {
   name: "YuMeiJie",
   age: 25
}

var array = ["string", 0, obj]
array.indexOf(obj) // 2
```

```javascript
var array = ["string", [0,1,2]]
array.indexOf([0,1,2]) // -1
```
