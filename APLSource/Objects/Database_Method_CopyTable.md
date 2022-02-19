# CopyTable Method

Applies to:{.prefix}

→[##.##.Database]{.info}

This method copies a table within a database.

The argument is composed of 2 items:

|-|-|
|1|Source table name|String|
| 2|Target table name|String|

The result is composed of 1 item:

|-|-|
|1|New table|Table object |

This method makes perfect copy of the table, maintaining foreign keys and the transaction history
of the rows. The primary and alternate key definitions are maintained. The new table may be
queried as of some time in the past, and will behave identically to the original table.

See the →[*.CopyTo] method to copy a table across databases, or to copy a table within a database
while purging inactive rows and de-referencing foreign keys.

