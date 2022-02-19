# CreateTable Method

Applies to:{.prefix}

→[##.##.Database]{.info}

This method creates a new table.

The argument is composed of 1, 2, or 3 items:

|-|-|
|1|Name|String|
|2|Structure|DataTable|
|3|Data|DataTable|

If only the name is provided, then an empty table with no rows and no columns is created. For example:

~~~
      t=d.CreateTable 'S'
~~~

To create a table with structure, first create a two-column DataTable to hold the column names and
columns types:

~~~
      ts=('Name' 'SNO,STATUS') ('Type' 'Char(2),Int32')
      t=d.CreateTable 'S' ts
~~~

To create and populate a table, create a DataTable to hold the column values as well:

~~~
      cv=('SNO' 'S0,S1,S2')('STATUS' (10 20 30))
      t=d.CreateTable 'S' ts cv
~~~

Creating a table with its structure at one time is much faster than creating a table with no
columns, and then repeatedly calling the →[*.Table.MethodList.CreateColumn] method.

