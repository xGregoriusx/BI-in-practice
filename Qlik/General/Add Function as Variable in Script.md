# Add Funciton as Variable in Script

## Use Case

Good practice is to keep complicated calculations in the script. Both to be able to easily comment and change calculations in one place (the script).

## Code

```
//In script define this variable:
//$1 means the field that will be used in Front End i.e. DateField.

SET MonthDiff = ((year(today())*12) + month(today())) - (((year($1)*12) + month($1)));

And then in an expression in the Front End:

=$(MonthDiff(DateField))
```
