# DeleteWhere Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method deletes rows in a table where a condition is true.

The argument is composed of 1 item:

|-|-|
|1|WhereClause|String|

The result is composed of 1 item:

|-|-|
|1|DeletedRowCount|Integer|

The result is the number of rows deleted. For example:

~~~
      D=Scripting.Server.OpenDatabase 'SandP'
      T=D.GetTable 'SP'
      T.DeleteWhere "QTY gt 200"
┌────┐
│6   │
└Int8┘
~~~

No rows will be deleted if there are any outstanding references to any single specified row. In
this case an error is signalled.

See also the →[Delete] method, for deleting rows by primary key value.

Note that in FlipDB, deletes are logical - no data is destroyed or overwritten. This allows for
historical queries, but it also means that deleting rows does not free up disk space. In fact, in
FlipDB deleting rows is as expensive as updating rows, and takes up additional space. In certain
circumstances, it is useful to physically delete rows, which may be accomplished with the →[Purge]
and →[PurgeWhere] methods.

