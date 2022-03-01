# GetItem Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

This method returns the value of specified cell.

The argument is composed of 2 items:

|-|-|
|1|Row|Integer|
|2|Column|String or Integer|

The result is composed of 1 item:

|-|-|
|1|Item|DataColumn|

In all cases for all objects, the result is a DataColumn. Index is zero-origin: use 0 for the
first item,  1 for the second item, etc. If the source data is simple, the result is a scalar. If
the source data is partitioned, the result is simple.

