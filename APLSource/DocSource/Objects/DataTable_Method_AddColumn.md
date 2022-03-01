# AddColumn Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

The argument is composed of two items:

|-|-|
|1|Name|String|
|2|Value|DataColumn|

The result is composed of 1 item:

|-|-|
|1|Instance|DataTable|

The AddColumn method changes the DataTable in place. An error is signalled if the column name
already exists in the table. The result is a reference to the same instance and is provided as a
convenience for passing it as an argument to an additional method.

