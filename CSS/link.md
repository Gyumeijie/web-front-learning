## The conventional rule of how pseudo-classes are arranged

The conventional rule which is also suggested in CSS specification is
```
:link —> :visited —> :hover —> :active (a.k.a LoVeHAte-order)
```

The four pseduo-classes has **same specificity**, but there is a priority order: `the latter one will override the former one`. :star:

For example, if hover was before visited, then hover wouldn't get ever applied, because it would be "forever" overwritten by visited style (if the linke has been really visited), that was applied after.

Same goes for :active (mouse down) - if it's defined before hover, then hover will always overwrite :active(mouse down)


But this isn’t the only useful order, nor is it in any way the “right” order, this is just **"conventional rule"** - it is not forced. The order in which you specify your pseudo-classes will depend on `the effects you want to show with different combinations of states.` :star:

## Links

[the order of pseudo classes for link](https://stackoverflow.com/questions/43118678/why-must-ahover-come-after-alink-and-avisited-in-the-css)
