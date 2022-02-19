# Index Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

This method sorts the rows of a DataTable.

The argument is composed of 1 item:

|-|-|
|1|Indices|Integer|

The result is composed of 1 item:

|-|-|
|1|Instance|DataTable|

All indices must be within the bounds of the number of rows of the table, and are in index-origin
0. Thus if a table has 4 rows, the indices must be drawn from the values 0, 1, 2 and 3. Indices
may be specified in any order, repeated, or elided.

