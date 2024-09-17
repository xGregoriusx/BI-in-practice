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
