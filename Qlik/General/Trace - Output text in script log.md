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
