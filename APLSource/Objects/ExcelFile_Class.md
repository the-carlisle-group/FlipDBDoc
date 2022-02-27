# ExcelFile Object

## Purpose

Provides access to Excel files and the Excel object model.

## Description

To open an existing Excel file, access the ExcelFile.New method from the Scripting object:

~~~
      XL=Scripting.ExcelFile.New 'myfile.xls'
~~~

The result is a FlipDB ExcelFile object.
Each ExcelFile object uses its own instance of Microsoft Excel,
which will have an entry in the Windows task manager.
The associated Excel instance is initially invisible,
but may be made visible by setting the Visible property to 1.
If the Excel instance is not visible, the instance is automatically closed when the
FlipDB ExcelObject goes out of scope (e.g., when a script finishes running).
If the Excel instance is visible, then it remains in existence even when
the ExcelFile object goes out of scope, and it is left up to the end user to close it.

The Open method may be used to open additional workbooks in the same FlipDB ExcelFile object,
and thus in the same associated Excel instance.

