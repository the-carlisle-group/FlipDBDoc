# PurgeWhere Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method PERMANENTLY removes all deleted and inactive rows in the table, as well as those active
rows in the table where a condition is true. This may affect the historical, but not the current,
integrity of the database; Inactive foreign key references may be lost. A side effect of this
method is that entire table's transaction id(the TWID)will be set to 1

The argument is composed of 1 item:

|-|-|
|1|WhereClause|String|

The result is composed of 1 item:

|-|-|
|1|PurgedRowCount|Integer|

The result is the total number of rows purged. For example:

~~~
      S=Scripting.Server
      J=S.DeleteDatabase 'SandP'
      D=S.BuildSuppliersAndParts ''
      T=D.GetTable 'SP'
      T.PurgeWhere 'QTY gt 300'
┌────┐
│3   │
└Int8┘
~~~

Note that an empty where clause will not select all rows (as is the case in a query), but rather no
rows, so:

~~~
      T.PurgeWhere ""
┌────┐
│0   │
└Int8┘
~~~

...results in a purge of all deleted and inactive rows, leaving all active rows.

See the →[Purge] method for a full discussion of how purging works.

No rows will be purged if any active rows specified in the where clause have any outstanding
references from another table. In this case an error is signaled.

