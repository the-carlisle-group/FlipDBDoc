# LaminateColumns Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

The argument is composed of 1 item:

|-|-|
|1|Data|DataTable|

And the result is composed of 1 item:

|-|-|
|1|Instance|DataTable|

This method adds the columns in the argument table that are not found
in the instance table, regardless of the →[*.Key] property.

Columns in the argument table are truncated or padded to match
the number of rows in the instance table.

To add new columns ordered by a key, see →[AddColumns].
