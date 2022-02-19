# UpdateRows Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

This method updates the existing rows of the DataTable.

The argument is composed of one item:

|-|-|
|1|Data|DataTable|

The result is composed of 1 item:

|-|-|
|1|Instance|DataTable|

The →[*.Key] property of both tables must be specified.

Updating rows to a DataTable is done in-place. The result of the UpdateRows method is a reference
to the same instance and is provided as a convenience for passing it as an argument to an
additional method.

