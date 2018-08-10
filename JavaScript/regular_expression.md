![selectors](../assets/regular_expression.svg)

## Usage

- match

```javascript
  var input = "aaabbbaaac"
  var result = input.match(/(\w)\1*/g)
  // ["aaa", "bbb", "aaa", "c"]
```
