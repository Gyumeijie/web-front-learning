## Equality Comparison


### The Abstract Equality Comparison Algorithm

The comparison `x == y`, where x and y are values, produces true or false. Such a comparison is performed as follows:

1. If Type(x) is the same as Type(y), then do further ***type-related*** judgment.
2. Otherwise, Type(x) is different from Type(y)
    1. If one is ***null*** and the other is ***undefined***, return **true**. :star: :star:
    ```javascript
      null == undefined  // true
      undefined == null  // true
    ```
    
    2. If one is ***String*** and the other is ***Number***, then converting the String to Number using `ToNumber`, then return the result of comparing two **Numbers**.
    ```javascript
      1 == "1" // true
      "1" == 1 // true
    ```
    
    3. If one is ***Boolean***, then converting the Boolean to Number using `ToNumber`, then return the result of comparing two **Numbers**.
    ```javascript
      false == 0   // true
      false = "0"  // true
      0 == false   // true
      "0" == false // true
    ``` 
    > string --> number, boolean --> number
    4. If one is ***Object***, and the other is either ***String*** or ***Number***, then converting the ***Object*** to Primitive using `ToPrimitive`, [more information](../types_and_grammar.md). :star: :star:
   ```javascript
     person = {
       name: "yumeijie",
       valueOf: function(){
             return true
       },
       toString: function(){
             return "yumeijie"
       }
    }
    
    person == true // true
    true == person // true
    person == "yumeijie" // false
    ```
   
### The Strict Equality Comparison Algorithm

The comparison `x === y`, where x and y are values, produces true or false. Such a comparison is performed as follows:

1. If Type(x) is the same as Type(y), then do further ***type-related*** judgment, but **no coercion** happens.
