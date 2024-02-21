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
