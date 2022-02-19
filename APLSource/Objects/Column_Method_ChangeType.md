# ChangeType Method

Applies to:{.prefix}

→[##.##.Column]{.info}

This method converts the data type of a column.

The argument is composed of 1 item:

|-|-|
|1|New data type|String|

|-|-|
|1|Instance|Column|

For example:

~~~
      C.ChangeType 'Float'
~~~

Demoting a column is disallowed if the demotion will result in a loss of data or precision.  System
column types may not be changed.

