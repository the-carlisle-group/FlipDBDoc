# Insert Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method adds one or more rows to the table.

The argument is composed of 1 item:

|-|-|
|1|Data|DataTable|

The result is composed of 1 item:

|-|-|
|1|KeyValue|Integer|

All of the rows in the argument →[*.Objects.DataTable] must not already exist in the table.

If the table is auto-keyed, the result is the first AutoKey value assigned, otherwise it is zero.

