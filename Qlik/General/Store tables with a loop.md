# Store tables with a loop 

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
