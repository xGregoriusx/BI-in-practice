# Comment Expressions

## Why?
When working in a Qlik App, there are a number of reasons to why you would want to comment your expressions.

- When you are building the expressions, there are reasons for _WHY_ you are building it the way you are. Stating those reasons, and assumptions will benefit the next person looking at the code. (Wheter it is someone else or you yourself ;) )
- Commenting _WHAT_ the code is set to do is always good, both for debugging reasons and to increase the speed and level of understanding for whomever is reading the code


## Example of commented expression

```
// Volume Effect Calculation
Num(
  //Calculate at product level
  Sum(Aggr(

  ( //Quantity difference over year
    Sum({$<Year={$(=Max(Year))}>}Quantity)
  -

```
