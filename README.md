# Checklist JS review

##### 1. Does each function do one thing and only one thing _(other than the "dispatchers")_

```
lol
```

- Does each anonymous functions are abstracted ?
- Does each complex conditions are abstracted ?
- Does the module contains a global description comment 
- Are the comments written useful ? 
- Does each function don't contain more than ~15 lines ?
- Does each function name are descriptive enough ?
    noProducts / hasNoProducts
- No for loop
- Only one level of indentation (max 2)
- Descriptive Variables
- One Liner return if (_.isEmpty($populatedBoxes)) { return; }
- No business logic in the variables
- Defensive IF     if (! compareSlotAvailable()) {
                displayCompareFullMessage();
                return false;
            }