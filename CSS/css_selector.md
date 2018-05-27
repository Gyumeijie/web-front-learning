# What is a CSS selector for?
CSS selectors is used to identify which elements you want to apply a style to when using the style element or an external stylesheet.

# CSS selectors
The following svg shows these css selectors.

![selectors](../assets/css_selector.svg)

# Note
### Class selector
There are two ways of expressing this selector: with and without the universal selector. The selectors **\*.classname** and **.classname** are equivalent. The first form is more descriptive, but the second form is the one that is most commonly used.

### Characteristics of selectors
Selectors of different type have their unique characteristic, for example, the ID selector is prefixed with "#", the class selector is 
prefixed with ".", and the attribute selector is enclosed by "[]" and so on. 

So, if we want to apply muliple selectors in one **css rule decalration**, we just simply concat these selectors, for example, we can write the following selector, of course it is a composite one:
```
.class1.class2[attr1="val1"][attr2="val2"]:hover::before
```

### Attribute selectors
Although the [attr~="val"] and [attr|="val"] selectors are most commonly applied to attribute which has mulitiple values, they doesn't
entail that; Also they can be used with sigle-value attribute. When matching these attribute selectors, the attribute named ```attr``` is split by ```" "``` for [attr~="val"] and ```"|"``` for [attr|="val"], and then check whether the value ```val``` is amongst these split values.

For example, the selector [lang|="en"] can match lang="en" or lang="en-".
