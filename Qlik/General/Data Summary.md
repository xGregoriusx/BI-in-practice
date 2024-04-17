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

Kudos to [Barnaby Dunn](https://au.linkedin.com/in/barnaby-dunn?trk=article-ssr-frontend-pulse_publisher-author-card)

[Source](https://www.linkedin.com/pulse/quick-qlik-trick-easy-data-summary-barnaby-dunn?trk=portfolio_article-card_title)
