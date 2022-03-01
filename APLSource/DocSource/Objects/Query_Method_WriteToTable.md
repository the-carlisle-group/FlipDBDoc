# WriteToTable Method

Applies to:{.prefix}

→[##.##.Query]{.info}

This method executes the query specified by the various properties of the Query object on the
database, and writes the result to a new table.

The argument is composed of 2 items:

|-|-|
|1|Database|String|
|2|Table|String|

The result is composed of 1 item

|-|-|
|1|New Table|Table  object|

The table specified by the argument must not exist.

This method is useful for running queries that return massive amounts of data that may not fit into
memory, and thus will not work using the →[Execute] method, which instantiates the entire result
of the query in memory using a DataTable.

All columns specified in the query must be visible.

