# DataTable Object

## Purpose

Represents a materialized, in-memory table.

## Description

A DataTable is an in-memory table that contains a collection
of [DataColumn]() objects. DataTables are returned as the result of a query from
the [Query.Execute]() method or the [Table.ExecuteQuery]() method.
A DataTable may also be created from scratch:

~~~
      ('SNO' 'S1,S2,S3') ('SNAME' 'Smith,Jones,Blake')
───────────────────────
 ┌SNO────┐  ┌SNAME──┐
 ↓S1     │  ↓Smith  │
 │S2     │  │Jones  │
 │S3     │  │Blake  │
 └Char(2)┘  └Char(5)┘
── 3 rows by 2 columns
~~~

Alternatively, an empty DataTable with no rows or columns may be created,
and then columns added one at a time:

~~~
      T=DataTable.New ''
      T.AddColumn 'SNO' 'S1,S2,S3'
      T.AddColumn 'SNAME' 'Smith,Jones,Blake'
~~~

The [GetColumn]() method returns a specific DataColumn
from the table,
by its name or position.
The [GetRow]() method returns a one-row DataTable from its row index.

The [DeleteColumns]() method deletes a column from the DataTable.
The [DeleteRows]() method deletes rows from the DataTable.
The [AddColumn]() method adds a column to the DataTable.
The column must conform to the table length,
and the column name must not already exist.
The [SetItem]() method sets the value of a cell by its
integer coordinates or by its row number and column name.

The [Evaluate]() method evaluates an expression in the context of
the DataTable and returns a DataColumn.
The [ExecuteQuery]() method executes a simple query on the
DataTable and returns a new DataTable.

The [RowCount]() and [ColumnCount]() properties return the number of rows
and columns respectively.
The [Changed]() property is a Boolean indicating whether or not any
changes have been made to the DataTable. These properties are all read-only.

A DataTable may be the argument to the [Table.Insert]() and
[Table.Update]() methods.

A DataTable tracks changes to itself made with the
DeleteRows, AddRows, AddColumn, and SetCell methods,
as well as the [SetItem]() method.
If the DataTable is the result of a query,
the Save method saves the accumulated changes to the associated
database Table,
that is, the starting Table of the original query.
The Save method will add rows, update rows, and delete rows as
necessary.
The Save method will signal an error if the DataTable is not
associated with a database Table.

