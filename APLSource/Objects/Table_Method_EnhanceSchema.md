# EnhanceSchema Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method reports on (and optionally makes) the schema changes necessary for a table to accept
data from another table.

The argument is composed of 2 items:

|-|-|
|1|Table|DateTable or Table|
|2|Action|Boolean|

The result is composed of 1 item:

|-|-|
|1|SchemaChanges|Char|

If the Action item is 0, no changes to the schema are made, and the result is a list of potential
changes. If the Action item is 1, the changes to the schema are actually made, and the result is a
list of actual changes.

