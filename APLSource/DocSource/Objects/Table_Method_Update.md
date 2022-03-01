# Update Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method updates the existing rows of the table.

The argument is composed of one item:

|-|-|
|1|Data|DataTable|

The result is composed of 1 item:

|-|-|
|1|Return code|0|

The primary key column must be provided. All key values must exist. Default fill values are used
for columns not provided. System columns may not be updated.

