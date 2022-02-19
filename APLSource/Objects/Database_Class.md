# Database Object

A Database object is usually obtained by one of three methods of the Server object:
CreateDatabase, OpenDatabase, and GetDatabase.
In addition, the server methods BuildSuppliersAndParts and BuildSampleLoan return
Database objects.
For example:

~~~
      D=Scripting.Server.OpenDatabase 'SandP'
~~~

The Name property reports the name of the Database, and is read-only.
To rename a Database, use the Server.RenameDatabase method.

The TablesNames property, also read-only, returns the names of tables contained
in the database.

Most of the methods of the Database object are for manipulating tables,
including CreateTable, CopyTable, GetTable, RenameTable and TableExists.

The Assign method applies one or more Assignment objects,
to update, insert or delete rows from one or more tables simultaneously.
See the Assignment object and the DeferredInsert, DeferredUpdate , and DeferredDelete methods of the
Table object for more details.

The WriteToFile method exports the entire database into a single .fdb file for
easy emailing, backup, or file transfer.

