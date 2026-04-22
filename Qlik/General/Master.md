# Master Qlik Sense

# Add Funciton as Variable in Script

Good practice is to keep complicated calculations in the script. Both to be able to easily comment and change calculations in one place (the script).

```
//In script define this variable:
//$1 means the field that will be used in Front End i.e. DateField.

SET MonthDiff = ((year(today())*12) + month(today())) - (((year($1)*12) + month($1)));

And then in an expression in the Front End:

=$(MonthDiff(DateField))
```

# Sorting a field as a Data Island

A data island is a table that is not connected to the overall data model.

I recently had a datamodel where a dimension table had the sorting field in it. Problem arose when the user filtered the data and looked at a measure that showed last years results. 
Since the sorting field was connected to the dimension table and the data did not contain all dimension values from the last year, the sorting did not work properly. Dimension values that had measure results from last year did not get the correct sorting.

The solution to this is to separate the sorting field from the data model and create a data island. By doing so, the sorting field is not connected to a specific year/dimension and by that works as intended.

Just create a table, Field_Sorting, and in the 'Sorting' of the field in front end set it so that it sorts on the field.

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

## Fix Dates 

```
Date(Num(Floor(DateField)), 'YYYY-MM-DD') as DateField
```



# Code Formatting

## Use Case

Formatting you Qlik script should not affect the output of your code, but it will help all human readers (including yourself) understand your code.

## Version History

**Why?**

To be able to follow changes and revisions done in the script.

**Code**

```
/**************************************************** App Name ****************************************************

 Purpose: This is the purpose of the app. Is it used as an Extract, Transform or Load application?

******************************************************************************************************************/


// ********************************************************************************************
// Version History
// ********************************************************************************************

SET Meta.Version = '1.0';

/*
-------------------------------------------------------------------------------------------------------------------
 Version:    1.0
 Date:       2024-09-12
 Developer:  Felix Marklund-Norén
 
 Comment:    Skapat första versionen av appen.
 
 Changes:    - Changed X to Y.
 			       - Changed section xyz.
 
*******************************************************************************************************************
-------------------------------------------------------------------------------------------------------------------
 Version:    X.X
 Date:       YYYY-MM-DD
 Developer:  Name Nameth
 
 Comment:    Copy this template to create a new entry in the Version History.
 
 Changes:    - Changed X to Y
 
*******************************************************************************************************************
*/
```

## 115 characters wide

**Why?**

If you make the lines of your code 115 characters long (or shorter) the code will be visible without horisontally scrolling when using a laptop screen with 1920 x 1080 resolution at 125% scale. 

Although you might generally be developing on a screen with higher resolution, this will ensure that frustration when debugging code on a laptop screen is kept to a minimum. :)

**Code**

```
/******************************************************************************************************************
	Code Section Name
*******************************************************************************************************************
	Description:
    What is this section used for? Dependencies?

******************************************************************************************************************/
```

## Data Connection Variables

**Why?**

By using a variable to define where something should be loaded (or stored) you can easily change the loaded folder if the file is moved, or if you need to change the file path for some other reason.

**Code**

```
//Path to load QVDs from.
SET vG.QVDPath_Load = lib://QVD/Extract QVD/;

//Path to store QVDs.
SET vG.QVDPath_Store = lib://QVD/Transform QVD/;
```

## Mapping Tables

**Why?**

Keep a separate code section specifically for Mappings. The Section should be early in the script, lik after Main section, since later sections need to use the mappings.

**Code**

```
MAP_CountryNames:                                   //Start table name with 'MAP_'
MAPPING LOAD                                        //Load Statements in capital letters
    CountryCodeID as [CountryCodeID],               //Always us alias (as) and define field name with brackets ('[]')
    CountryName   as [CountryName]                  
FROM                                                //Load Statements in capital letters
    [$(vG.QvdPathSource_QVSync)SUS_Org.qvd] (qvd)   //Use variable for folder path, and keep on separate line
;                                                   //Keep closeing statement ';' on separate line to increase readability

                                                    //Separate Mapping Loads woth two empty rows
MAP_CountryNames:                                   
MAPPING LOAD                                        
    CountryCodeID as [CountryCodeID],               
    CountryName   as [CountryName]                  
FROM                                                
    [$(vG.QvdPathSource_QVSync)SUS_Org.qvd] (qvd)   
;                                                   

```

## Load Tables

**Why?**

Keep a clear formatting for load tables, in order to increase comprehension and readability.

**Code**

```
TableName:                                  //Always define a table name. Be explicit.
LOAD DISTINCT                               //Load Statements in capital letters
    Field1              as [Field1],        //Always us alias (as) and define field name with brackets ('[]')
    Mid(Field2, 6, 200) as [Field2],
    False()             as [Field3],
    -1                  as [Field4]
RESIDENT                                    //Load Statements in capital letters
    DataTable                          
WHERE
    Left([ConditionField], 4) < 'Example'   //Space between characters such as < > + - / *
ORDER BY
    [OrderField]                            //Keep on separate row
;                                           //Keep closeing statement ';' on separate line to increase readability

CONCATENATE (TableName)                     //One empty row if Concatenate, Keep or Join is used, otherwise two empoty rows.
LOAD DISTINCT                               
    Field1              as [Field1],        
    Mid(Field2, 6, 200) as [Field2],
    False()             as [Field3],
    -1                  as [Field4]
RESIDENT
    DataTable                          
WHERE
    Left([ConditionField], 4) < 'Example'
    and [Conditionfiel2] = 'yes'
ORDER BY
    [OrderField]                            
;
```

## List of Capitalized Load Statements

**Why?**

Keep these statements capitalized to increase readability and comprehension.

**List**

```
LOAD
DISTINCT
FROM
INLINE
RESIDENT
AUTOGENERATE
WHERE
GROUP BY
ORDER BY
CONCATENATE
JOIN
DROP TABLE
DROP FIELD
```

# Comment Expressions

## Why?
When working in a Qlik App, there are a number of reasons to why you would want to comment your expressions.

- When you are building the expressions, there are reasons for _WHY_ you are building it the way you are. Stating those reasons, and assumptions will benefit the next person looking at the code. (Wheter it is someone else or you yourself ;) )
- Commenting _WHAT_ the code is set to do is always good, both for debugging reasons and to increase the speed and level of understanding for whomever is reading the code


## Example of commented expression

```
// Summed sales for 2015 requested by name, customer.
Sum({<Year={2015}>}Sales)

/*
Modified by Person Pearson the 14th nov 2023
*/
```

# Create Empty Table for Looping

## Use Case

When creating a loop in the script you often do so in order to loop over a subset of data in order to create a new table.

That could be solved by creating a lot of temp tables in the loop, concatenating those temp tables into one and then dropping all the temp tables.

But another way is to start by creating an 'empty' table and then concatenating the data in the loop to that (empty) table.


## Code

```
LoopTable:
Load 
	0 as %KEY_ID
AutoGenerate 0; //Autogenerate 0 creates the empty table.

for iteration = 0 to 5
    
    Concatenate (LoopTable)
    Load
    	$(iteration) as %KEY_ID,
        Field1,
        Field2
    Resident DataTable;
    
next

```

# Data Summary

## Use Case

To easily get an overview of what files has been loaded, their folder paths, number of rows etc.

## Code

```
/*------------------------------------------------------------------------------
	Create an empty table and a SUB routine to fill the table with data.
------------------------------------------------------------------------------*/
DataSummary:
Load 
	0 as [Data Table]
AutoGenerate 0;

SUB AppendDataSummary(pTableName)

Concatenate(DataSummary)
Load
	[Data Table],
	[Data Source],
	[Data Path],
	Date(Max([Last Modified]), 'dd-mm-yyyy') as [Last Modified],
	Max([Rows Loaded])                       as [Rows Loaded File],
	NoOfRows('$(pTableName)')                as [Rows Loaded Total]
Resident [$(pTableName)]
Group By [Data Table], [Data Source], [Data Path]
;

Drop Fields [Data Table], [Data Source], [Data Path], [Last Modified], [Rows Loaded] from $(pTableName);

END SUB

/*------------------------------------------------------------------------------
	Load files and call the SUB routine.
------------------------------------------------------------------------------*/

File1:
LOAD
	Key1,

	'File1a'   as [Data Table],
	FileName() as [Data Source],
	FilePath() as [Data Path],
	FileTime() as [Last Modified],
	RecNo()    as [Rows Loaded]
FROM [lib://folder1/file1a.xlsx]
(ooxml, embedded labels, table is Sheet1);

Concatenate(File1)
LOAD
	Key1,

	'File1b'   as [Data Table],
	FileName() as [Data Source],
	FilePath() as [Data Path],
	FileTime() as [Last Modified],
	RecNo()    as [Rows Loaded]
FROM [lib://folder1/file1b.xlsx]
(ooxml, embedded labels, table is Sheet1);

Call AppendDataSummary('File1')


File2:
LOAD
	Key2,
	
	'Fiel2'    as [Data Table],
	FileName() as [Data Source],
	FilePath() as [Data Path],
	FileTime() as [Last Modified],
	RecNo()    as [Rows Loaded]
FROM [lib://folder/file2.qvd]
(qvd);

Call AppendDataSummary('File2')
```

## Example Output




## Source

[Source](https://www.linkedin.com/pulse/quick-qlik-trick-easy-data-summary-barnaby-dunn?trk=portfolio_article-card_title)


 # Hiding fields from Front End

## Why?
Often you'll want to create fields that you don't want to show to the user. Key fields are a common example. Another is if you want to have a field that acts as a selector (show/hide chart in a container for example). 
Then you'll want to hide that field from the end user, both so that they don't have to see the selection in the field and also not to clutter up the selectable fields if the user will build their own charts in a sheet.

## How can you hide fields?
There are two ways in which you can hide a field. 

- Set the system variable HidePrefix to a character and then add that character to the start of the name of the fields you want to hide.

> It is common the rename Key Fields as %KEY_fieldname, which then will be hidden by setting HidePrefix to %.

```
// Hides fields which name starts with %.
SET HidePrefix = %;

test:
LOAD
	field as %hidden_field
FROM [lib://Data/Dataset.xlsx]
(ooxml, embedded labels, table is table);
```

- Tag a field with '$hidden' to hide the field.

```
Tag Field 
  first_field,
  second_field
with '$hidden';
```

## Which way should I use?
The first alternative, HidePrefix is a good alternative to "always use" since you'll have key fields in your data model that you'll want to hide from the end user.

The second alternative, tags, are good to use when you explicitly want to show (yourself or other developers) that these fields are hidden. A tip in that case is to put a section of your Qlik code att the end named 'Hide Fields' where you put the code.
Then it is easy for the developer to comment out that part of the code if it is necessary to unhide the fields.

> When you hide fields and beforehand select the setting 'Always one selected' for the field this will be shown in Qlik Front End
> ![image](https://github.com/xGregoriusx/BI-in-practice/assets/139049888/54f8bc31-bed6-431b-9c54-713dc2996d47)

# Inline Load 

## Use Case

An Inline Load can be used when you want to create a table where you can define fields and field values directly in the Load Script.



## Code

```
TableName:
LOAD * Inline [
  FieldName1,     FiledName2
  FieldValue1_1,  FieldValue2_1
  FieldValue1_2,  FieldValue2_2
  FieldValue1_3,  FieldValue2_3
];
```

## Example Output

![image](https://github.com/xGregoriusx/BI-in-practice/assets/139049888/f1e816d9-e8f1-45de-82b6-abcea0ed4108)

# Inline FROM Load

## Why?
If you want to load data via an Inline table and then do transformations on the data this can be done by making an Inline table and then doing a resident load from that table and making the transformations you want.

But, if you want to, you can make all steps into one by loading a table with transformations and loading the table data directly from an Inline table.

## Example of loading transformated data from an Inline table

```
Product:
Load
    Region & '|' & ProductId as %Region_Product,
    ProductId,
    Price
Inline [
    Region, ProductId, Price
    North,  A,         5
    East,   A,         5
    West,   A,         6
    North,  B,         13
    East,   B,         8
    West,   B,         9
    North,  C,         1
    East,   C,         1
    West,   C,         1
    North,  D,         21
    East,   D,         12
    West,   D,         7
];
```

## Output

![image](https://github.com/xGregoriusx/BI-in-practice/assets/139049888/3268fce9-db21-4ef7-a1d4-49f4c1979e0c)

# Optomized Load

## Use Case
Data can be loaded in two different ways in Qlik. 'Standard' or as an Optomized load. The way in which data is loaded is selected automatically by the Qlik engine, 
depending on if your Load statement follows the requirements for an optomized load or not.

When possible it is a good idea to code your load statement in a way that becomes an optomized load, but it is especially relevant when loading large amounts of data. 
(Think loading a QVD with 100 million rows of data.) In those cases the load time can be significantly shortened by doing optomized loads, when possible.

## Rules for Optomized Load
A good rule of thumb is to think that *no transformations are allowed* for an optomized load.

But there are more things to think about:

### Allowed for Optomized Load

- Renaming/aliasing is allowed

```
LargeTable:
LOAD
  DateField   as Date
  SalesField  as Sales
FROM LargeDataset;
```

- Using a single field in WHERE Exists()

```
LargeTable:
LOAD
  DateField   as Date
  SalesField  as Sales
FROM LargeDataset
WHERE Exists(DateField);
```

- Using a DISTINCT LOAD

It is important to remember that the DISTINCT will be processed *after* the LOAD. Meaning that all data will be loaded and then a Distinct of that data will be made.

```
LargeTable:
LOAD DISTINCT
  DateField   as Date
  SalesField  as Sales
FROM LargeDataset;
```

### Not Allowed for Optomized Load

- Transformation is not allowed

```
LargeTable:
LOAD
  DateField                  as Date
  SalesField                 as Sales
  SalesField - DiscountField as NetSales //This is a transformation
FROM LargeDataset;
```

- Adding a new field is not allowed

```
LargeTable:
LOAD
  DateField   as Date
  SalesField  as Sales
  1           as DataSource // Adding a new field
FROM LargeDataset;
```

- Filters in a WHERE CLAUSE is not allowed

```
LargeTable:
LOAD
  DateField   as Date
  SalesField  as Sales
FROM LargeDataset
WHERE DateField = '2006-02-19';
```

- Using multiple fields in WHERE Exists() is not allowed

```
LargeTable:
LOAD
  DateField   as Date
  SalesField  as Sales
FROM LargeDataset
WHERE Exists(DateField, SalesField);
```


## How to know when Optomized Load is used?

If the requirements for an Optomized Load are fulfilled Qlik will print *(QVD (row-based) optomized)* in the script log when running the script.

![image](https://github.com/xGregoriusx/BI-in-practice/assets/139049888/8af6a404-d0da-418b-b6ae-e7fe68eff001)

## Scripting Ideas for Large Datasets

It is often a good idea to first load an entire table as an optomized load, and *then* do the needed transformations by using RESIDENT LOAD.
As opposed to doing one (non-optomized) load where you directly make the needed transformations.

In this example the goal is to load all distinct years from a date field in a dataset with aorund 113 million rows.

The 'regular' way to do it would be by loading the data DISTINCT and doing the year transformation as a single load statement.
Since we are transforming the DateField into years we are breaking the rules for an Optomized Load.

```
NonOptomizedLoad:
LOAD DISTINCT
  Year(DateField) as Year
FROM LargeDataset;
```

This takes about 2.28 minutes.

A better way would be to first load the data as an Optomized Load, and then do a RESIDENT LOAD and make the necessary transformation.

```
OptomizedLoad:
LOAD DISTINCT
  DateField as Date
FROM LargeDataset;

FinalTable:
LOAD DISTINCT
  Year(Date) as Year
RESIDENT OptomizedLoad;

Drop Table OptomizedLoad;
```

The fastest load doing it this way took 18 seconds. Or around 12% of the time of the original script.

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

# Random Number Picker without Duplicates

## Use Case

If you want to randomly pick values from a field that can be solved by using Rand().
But then it is possible to get the same pick numerous times.

If you instead want to randomly select values, without getting duplicate values, this is how you do it:

## Code

```
/*------------------------------------------------------------------------------
Create a list of rows to pick from.

Outside of this example, this would be the table you want to pick rows from.
------------------------------------------------------------------------------*/
List:
Load
	RecNo() as Rows
AutoGenerate 10;

/*------------------------------------------------------------------------------
Variables that controls which field the script will pick from, and which table.
------------------------------------------------------------------------------*/

SET vField = Rows;
SET vTable = List;

/*------------------------------------------------------------------------------
Get the size of the List table and define the variable vSize.
------------------------------------------------------------------------------*/
Size:
Load
	Count($(vField)) as Size
Resident '$(vTable)';

LET vSize = Peek('Size');
Drop Table Size;

/*------------------------------------------------------------------------------
Put together the field AllRows, which will look like '1|2|3|...'.
------------------------------------------------------------------------------*/
CompleteList:
Load
	Concat($(vField), '|', $(vField)) as AllRows
Resident '$(vTable)';

LET vAllRows = Peek('AllRows') & '|';
Drop Table CompleteList;

/*------------------------------------------------------------------------------
Load a table that randomly picks out values from 'AllRows'.

PickNo 		- Pick number...
Remaining 	- Shows the remaining numbers possible to be picked.
PickedRow 	- The row that has been (randomly) picked.
RandomPick 	- The order of the element of vAllRows that has been picked.

'Autogenerate $(vSize)' determines that the table Picks should pick a random 
number until all numbers have been chosen. But this can be set to a specific 
amount, if you want less picks than all possible picks.
------------------------------------------------------------------------------*/
Picks:
Load
	RecNo()                                                                              as PickNo,
	Replace( If(RecNo()=1, '$(vAllRows)', Peek('Remaining')),
	SubField(If(RecNo()=1, '$(vAllRows)', Peek('Remaining')), '|', RandomPick) & '|','') as Remaining,
	SubField(If(RecNo()=1, '$(vAllRows)', Peek('Remaining')), '|', RandomPick)           as PickedRow,
	RandomPick;
Load
	Ceil(Rand() * ($(vSize) - (RecNo()-1)))                                              as RandomPick
AutoGenerate $(vSize);

Drop Fields RandomPick, Remaining;

/*------------------------------------------------------------------------------
Cleanup of variables.
------------------------------------------------------------------------------*/
LET vSize    =;
LET vAllRows =;
```

## Example Output

![image](https://github.com/xGregoriusx/BI-in-practice/assets/139049888/5ec024c1-72f5-4234-aff8-876c11331a96)


## Source

Kudos to [petter](https://community.qlik.com/t5/user/viewprofilepage/user-id/41481)

[Source](https://community.qlik.com/t5/user/viewprofilepage/user-id/41481](https://community.qlik.com/t5/QlikView-App-Dev/Random-numbers-without-repeating/td-p/127315)https://community.qlik.com/t5/QlikView-App-Dev/Random-numbers-without-repeating/td-p/127315)

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

# Set Analysis Cheat Sheet

![image](https://github.com/xGregoriusx/BI-in-practice/assets/139049888/58c75722-01ec-4eb3-9318-004cbce02610)

# Store Tables with a Loop 

## Use Case

If you want store the tables that have been loaded into a Qlik app you could write specific store statements for each table.
But that is not a dynamic solution, and if there are a lot of tables it will take time writing all store statements. 
Instead you can use a loop to store all tables into a specific data connection:

## Code

```
// For each table
for i = 0 to NoOfTables() - 1

  // Define variable as the name of the specific table
  Let tableName = TableName($(i));

  // Store the table with the name of the variable into data connection
  Store [$(tableName)] into lib://MyDataConnection\Path\to\some\folder\$(tableName).qvd;
next

```

# Trace - Output text in script log

## Use Case

When running a script i Qlik, Trace can be used to output text in the script log. 
This serves the purpose both of being used for trouble shooting the script when it fails, as Trace can output messages in different parts of the script
but also to inform the developer that is reloading the script what is happening during the load.

## Code

```
trace |---- Started loading data ---|;

DataTable:
Load
	'test' as Field
AutoGenerate 1;

Drop Table DataTable; trace >>> Dropped Table DataTable;
```

Output:

![image](https://github.com/xGregoriusx/BI-in-practice/assets/139049888/fa79dfad-807b-45b4-ad88-6ed8e18b9922)

# Use Variable as Folder Path

## Use Case
When loading data into Qlik you often use folder paths that in its entirety or in part is reused many times when loading (or re-lodaing) different tables. 
When you then need to change one, or several, of the folder paths you have quite a lot of work to do which is prone to error. 

Instead you can define a variable that contains the relevant folder path and then just have to change the variable when the folder path needs to be changed.

## Code

In the Main-section of your code, add a variable:

```
SET vDataPath = 'lib://Datafiles/Data/Test_Path/';
```

Then in later code sections expand the variable in the FROM line of code:

```
FROM [$(vDataPath)Products.xlsx] 
```
