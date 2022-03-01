# WriteToFile Method

Applies to:{.prefix}

→[##.##.Database]{.info}

This method creates a .fdb file, a compressed copy of the database, suitable for easy emailing or
other file transfer methods.

The argument is composed of 1 item:

|-|-|
|1|Target file name|String|

The result is composed of 1 item:

|-|-|
|1|Return code|0|

For example:

~~~
      D.WriteToFile 'c:olderSuppliers and Parts.fdb'
~~~

A .fdb may be opened read-only using the static method →[*.OpenFile].

