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

