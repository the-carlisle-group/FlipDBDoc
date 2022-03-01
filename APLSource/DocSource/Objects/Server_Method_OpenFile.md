# OpenFile Method22222

Applies to:{.prefix}

→[##.##.Server]{.info}

Notes

Opens a .fdb file in read-only mode.

The argument is composed of 1 item:

|-|-|
|1|File name|String|

The result is composed of 1 item:

|-|-|
|1|Database|Database object|

This is a static method, and may be called directly from the Server class. For example, where S is
a Server:

~~~
      D=S.OpenFile 'Database'
      F='c:olderSuppliers and Parts.fdb'
      D.WriteToFile F
      D2=#.FlipDB.flipdb.Server.OpenFile F
~~~

Note that D2 is a →[*.Objects.Database] object like D, but that D2 is open and exists
independently of the server.  D2 may be queried like any database, but is read-only, and may not
be changed in any way. There is no way to open a .fdb file in a read/write mode.

See also: →[*.WriteToFile]

