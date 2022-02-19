# AddColumns Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

The argument is composed of 1 item:

|-|-|
|1|Data|DataTable|

And the result is composed of 1 item:

|-|-|
|1|Instance|DataTable|

This method adds the columns in the argument table that are not found in the instance table. If
both tables have the →[*.Key] property specified, then the new columns are aligned appropriately.
If either table does not have the Key property specified, then no
rows are considered to match.
Values for non-matching rows are populated with blanks or zeros depending on the data type.

To add new columns without regard to keys, see →[*.LaminateColumns].
