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
