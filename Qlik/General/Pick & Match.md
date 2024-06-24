# Pick & Match

## Why?
Using Pick & Match solves the same problem as using nested If-statements (If A, Do B, If C, Do D etc.), but is either as fast or faster than suing a lot of If-statements.

From what I gather as a rule of thumb Pick & Match is a better choice (performance-wise) than using nested If-statements.

## Example of nested If and the Pick & Match

```
//Nested If

If(ProductId='A', 'Area A',
If(ProductId='B', 'Area B',
If(ProductId='C', 'Area C',
'Else Condition Value')))               as NestedIf_example


Pick(Match(ProductId, 'A', 'B', 'C')+1
    'Else Condition Value',
    'Area A',
    'Area B',
    'Area C')                           as PickMatch_example

```

## Output
(Same for both examples, difference is load time depending on data amount)
![image](https://github.com/xGregoriusx/BI-in-practice/assets/139049888/3c6ca252-9c1b-495f-a740-41b9df7abdd5)
