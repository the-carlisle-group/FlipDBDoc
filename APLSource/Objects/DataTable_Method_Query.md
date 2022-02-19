# Query Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

This method creates a →[*.Objects.Query] object, which may then be used to query the database.

The argument may be void, or the argument may be composed of 2 items:

|-|-|
|1|Where clause|String|
|2|Select clause (Column names)|StringList|

The result is composed of 1 item

|-|-|
|1|Query Object|Query object|

The Where and Select parameters may be an empty string. The Where and Select parameters are
instantiated as the Where and Select properties of the resulting Query object.

