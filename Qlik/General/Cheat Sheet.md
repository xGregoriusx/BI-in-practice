# Cheat Sheet

## Comment

```
/*------------------------------------------------------------------------------ 

------------------------------------------------------------------------------*/ 
```

## Peek() 

```
Peek('field_name', row_no, 'table_name');  

Peek('Field', $(i), 'Table'); 
```

## SET FilePath 

```
SET QVDPath = 'lib://MyLib/Folder1/Folder2/';
```

## SubField() 

SubField(Field, 'separator', cell_no) 
 
```
SubField(Field, ',') 

SubField(Name, ' ', -1) //-1 outputs the last cell of the formula.. 
```

## Replace(), PurgeChar(), KeepChar() 

Replace – Change all instances of a string/character to another string. 
Replace(field, from_string, to_string) 
Replace(Name, 'Anders', 'Stefan') 

PurgeChar – Remove all instances of a string/character from another string. 
PurgeChar(text, remove_characters) 
PurgeChar(Name, '_') 

KeepChar - Only keeps a substring of the complete string. 
KeepChar(text, keep_characters) 
KeepChar(Email, '@') 

## Empty Loop-table

Generate an empty loop-table that can be filled with rows from a loop. Note that the field of the empty table will be in the "final" table, 
so either name it as a field that comes from the looped data, or drop the field when tha rows have been concatenated.

```
LoopTable: 
Load 
    0 as ID 
Autogenerate 0;
```

## For-Loop 

```
for i = 0 to NoOfRows('Table') - 1

  LET vField1 = Peek('Field1', $(i), 'Table'); 
  LET vField2 = Peek('Field2', $(i), 'Table'); 

  tmp_Fakta
  Load
    * 
  FROM [$(QVDPath)Table.qvd] (qvd);

next
```

## Clean up variables/tables/fields

```
Variables: 
LET i =;
LET vTable =;

Tables: 
Drop Table tmp_table; 

Fields: 
Drop Field tmp_field; 
```

## Order By 

Sorts table based on one or several fields. Note that it can only be used on Resident Loads.
 
```
Table: 
Load 
    * 
Resident tmp_Table 
Order By Field1 asc, Field2 desc 
;
```

## ApplyMap() 

Maps values from a field, via a key field. Acts like a Join, but only for one field.

```
//This has to be declared first.
MAP_CountryCode: 
MAPPING LOAD
  [Countrycode], 
  [Country Name] 
FROM [Lib://Countries.QVD] (qvd) 
; 

//Put this in the load statement when loading your final table.
ApplyMap('MAP_CountryCode', Country) as [Country Name] 

ApplyMap('map_tablename', key_field )
```

## New code section in "flowing text" 

```
///$tab Name NewTab 
```

## Expand field value in Expression 

```
$(=GetFieldSelections(Fält)) 
```

## Colormix1 Gradient 

```
Colormix1( 
(([Field]) - Min(total [Field])) / (Max(total [Field]) - Min(total [Field])), 
Black(),  
LightRed()) 
```
 
## Basic Master Calendar 

```
SET vDate = %KEY_Date;

MasterCalendar:
LOAD
	TempDate                        as $(vDate),
	
	Year(TempDate)                  as Year, 
	Month(TempDate)                 as Month,
	'Q' & Ceil(Month(TempDate) / 3) as Quarter,
;

//=== Generate a temp table of dates === 
LOAD
	Date(MinDate + IterNo())        as TempDate
WHILE MinDate + IterNo() <= MaxDate;

//=== Get min/max dates from Date field ===/
LOAD
	Min(FieldValue('$(vDate)', Recno()))-1 as MinDate,
	Max(FieldValue('$(vDate)', Recno()))   as MaxDate
AUTOGENERATE FieldValueCount('$(vDate)');
```
 
