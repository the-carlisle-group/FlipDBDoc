# Key Property

Applies to:{.prefix}

→[##.##.DataTable]{.info}

This property specifies the name of one or more columns. The Key property is an implicit argument
referenced by any DataTable method that requires a row lookup between two DataTables, inlcuding
→[*.DataTable.MethodList.AddRows], →[*.DataTable.MethodList.UpdateRows], and →[*.DataTable.MethodList.AddColumns].

The Key property of a DataTable is distinct from the primary key, alternate key, and foreign key
concepts of a Table object, though it may represent them.

A DataTable does **not** enforce uniqueness on a column named as the key column, as the column may or
may not represent a primary key, and uniqueness may not be appropriate.

When setting the Key property, the columns specified must exist. However, when referencing the Key
property,  the columns specified may or may not exist. Thus methods that implicitly use the Key
property, like AddRows, may signal a "Key column not found" error.

If either one or both tables has no key specified, then no rows are considered to match. Otherwise,
the key columns specified for each table must correspond in number and type, or an error will be
signalled. The actual names of the key columns do not need to match, so they must be specified in
the proper order. For example the Key 'SNO,PNO' is NOT the same as 'PNO,SNO'.

The default value of the Key property of a new instance of a DataTable is empty. However, if the
DataTable is the result of a database query, and the query result includes the primary key column
of the starting table, then the Key property defaults to the name of the primary key column.

A side effect of setting the Key property is to clear all pending changes in the DataTable,
resetting the Changed property to 0

