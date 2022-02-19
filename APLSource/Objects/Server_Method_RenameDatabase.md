# RenameDatabase Method

Applies to:{.prefix}

→[##.##.Server]{.info}

This method renames a database.

The argument is composed of 2 items:

|-|-|
|1|Source database name|String|
| 2|Target database name|String|

The result is composed of 1 item:

|-|-|
|1|Return code|0|

For example:

~~~
      S.RenameDatabase 'SANDP' 'SuppliersAndParts'
~~~

