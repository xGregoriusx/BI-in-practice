# Replace multiple characters in string (MapSubstring)

## Use Case
Sometimes you want to replace different parts of a string in a field. For instance you want to save QVDs with a name that comes from a field value, 
but said field value contains characters that is not supported to use in file names, or perhaps you have bad source data but can't fix it more upstream and want to do it insted in a Qlik Transform app.

What you can do is use Replace() to change specific characters

```
Replace(Field, '/', '_') as ReplacedField
```

The problem is if you want to substitute a lot of different characters, then you _could_ stack Replace() on Replace() on Replace() etc.

A more suitable way to solve it is by making an Inline table with two fields, one with the character you want to replace and one field with the character that should be used instead.
Then you use MapSubstring() when declaring the field in your Load Statement (kind of like an ApplyMap() but for string replacement).

## Example code

```
/*
First make the Inline table where you specify characters that should be
replaced and which characters they should be replaced for.
*/

map_Textreplacement:
Mapping Load * Inline [

  Character, ChangeTo
  '+-',      'plusminus',
  '+', 	     'plus',
  '-',	     '_',
  ' ',	     '_',
  '/',	     '',
];


/*
Then in your Load Statement use MapSubstring().
*/

Table:
Load
  MapSubstring('map_Textreplacement', Field) as ReplacedField
Resident Data;

```
