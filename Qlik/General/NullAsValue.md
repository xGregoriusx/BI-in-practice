# NullAsValue

## Why?
Null is generally concernad as representing a 'missing' value. But sometimes you want to exchange a null value for something else.

For instance in a financial app you might want to substitute null for 0 when dealing with fields for financial outcome or budget.

## Example

```
//Defines which fields should be affected.
NullAsValue OUTCOME, BUDGET, PROGNOSIS;

//Null values for the fields above should be substituted for 0.
Set NullValue = 0;

```

## Output
