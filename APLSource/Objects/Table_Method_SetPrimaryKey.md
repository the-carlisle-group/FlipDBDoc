[DATA]:index='Methods ⋄ SetPrimaryKey ⋄ SetPrimaryKey ⋄ Table Method'

# SetPrimaryKey Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method specifies the primary key of a table.

The argument is composed of 1 item:

|-|-|
|1|ColumnName|String|

|-|-|
|1|Instance|Column|

The column values must be unique, and may be of any data type except Blob. If the column is already
part of an alternate key, the alternate key specification is removed. The argument may be AUTOKEY,
to set the primary key to the built-in auto-incrementing column.

