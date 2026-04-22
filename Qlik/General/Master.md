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



# Example headings

## Sample Section

## This'll be a _Helpful_ Section About the Greek Letter Θ!
A heading containing characters not allowed in fragments, UTF-8 characters, two consecutive spaces between the first and second words, and formatting.

## This heading is not unique in the file

TEXT 1

## This heading is not unique in the file

TEXT 2

# Links to the example headings above

Link to the sample section: [Link Text](#sample-section).

Link to the helpful section: [Link Text](#thisll-be-a-helpful-section-about-the-greek-letter-Θ).

Link to the first non-unique section: [Link Text](#this-heading-is-not-unique-in-the-file).

Link to the second non-unique section: [Link Text](#this-heading-is-not-unique-in-the-file-1).
