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
