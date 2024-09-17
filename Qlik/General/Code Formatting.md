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
