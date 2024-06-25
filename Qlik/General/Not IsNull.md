# Not IsNull

## Use Case

One use case is if you have a field with cells that either have values or not, and you want to create a flag field that states if the field is null or not.

## Code

```
If(Not IsNull([Field]),1) as Flag_NotNull
```
