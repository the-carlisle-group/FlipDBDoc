# AddRows Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

This method adds "new" rows to the DataTable as determined by the →[*.Key] property.

The argument is composed of one DataTable:

|-|-|
|1|Data|DataTable|

Or one or more DataColumns:

|-|-|
|1|Data|DataColumn(s)|

The result is composed of 1 item:

|-|-|
|1|InstanceTable|DataTable|

If the argument is a DataTable, it may contain any number of columns in any order. If no Key
property is specified in either or both of the tables, then all rows are considered "new" and are added.

If DataColumns are provided, they must be provided in the exact order and number of the instance
DataTable. In this case, the Key of the DataTable implied by the columns  is assigned the Key
value of the instance DataTable.

Adding rows to a DataTable is done in-place. The result of the AddRows method is a reference to the
same instance and is provided as a convenience for passing it as an argument to an additional method.

