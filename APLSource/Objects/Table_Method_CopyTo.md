# CopyTo Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method copies a table across databases or within a database.

The argument is composed of 2 items:

|-|-|
|1|Target database|Database instance|
| 2|Target table name|String|

The result is composed of 1 item:

|-|-|
|1|New table|Table object |

This method copies the current active rows of the table into a new table. The transaction history,
including deleted and inactive rows, is lost. In additional, foreign keys in the table are
de-referenced. The primary and alternate key definitions are maintained.

See the →[*.Database.MethodList.CopyTable] method to copy a table within a database while
maintaining foreign keys and transaction history.

