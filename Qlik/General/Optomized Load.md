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
