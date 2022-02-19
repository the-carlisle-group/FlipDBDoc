# Purge Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method !!permanently!! removes all deleted and inactive rows in the table,  as well as the
rows specified by the key values in the argument. This may affect the historical (but not the
current) integrity of the database; historical foreign key references may be lost. A side effect
of this method is that entire table's transaction id (the TWID) will be set to 1.

The argument is composed of 1 item:

|-|-|
|1|KeyValues|DataColumn or DataTable|

The result is composed of 1 item:

|-|-|
|1|PurgedRowCount|Integer|

Every update to a row creates a new active row, and turns an existing row inactive. Thus, updating
two rows creates two inactive rows:

~~~
      S=Scripting.Server
      J=S.DeleteDatabase 'SandP'
      D=S.BuildSuppliersAndParts ''
      T=D.GetTable 'SP'
      T.Update ('AUTOKEY' (1 2))('QTY' (500 600))
┌────┐
│2   │
└Int8┘
~~~

Which may be physically removed with the Purge method:

~~~
      T.Purge emptyInteger
┌────┐
│2   │
└Int8┘
~~~

For each row logically deleted in FlipDB, two physical rows become inactive. So deleting 3 rows:

~~~
      T.Delete 3 4 5
┌────┐
│3   │
└Int8┘
~~~

...creates 6 inactive rows which may be physically removed with the Purge method:

~~~
      T.Purge emptyInteger
┌────┐
│6   │
└Int8┘
~~~

Active rows may be purged by specifying their key values. Note that the Purge method always purges
all deleted and inactive rows in addition to any active rows explicty specified in the argument. In
this case below, 4 physical rows are purged, 2 for the deleted row and 1 each for the two explicty
purged in the argument:

~~~
      T.Delete 6
┌───────┐
│1      │
└Boolean┘
      T.Purge 11 12
┌────┐
│4   │
└Int8┘
~~~

No rows will be purged if any active rows specified in the argument have outstanding references
from another table. In this case an error is signalled.

See also the →[*.Table.MethodList.Delete], →[*.Table.MethodList.DeleteWhere], and
→[*.Table.MethodList.PurgeWhere] methods.

