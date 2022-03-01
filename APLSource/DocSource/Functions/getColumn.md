# getColumn

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Gets a Column as a DataColumn.{.purpose}

~~~
R=[Y] getColumn X
~~~
Y is an optional Table or DataTable. If Y is provided, X must be a simple name.
If Y is not provided, X may be in the form 'ColumnName', 'TableName.ColumnName'
or 'DatabaseName.TableName.ColumnName'. If the database name or table name
is ommited, they are determined by the context.

## Examples
~~~
      getTable 'SandP.P.WEIGHT'




~~~
