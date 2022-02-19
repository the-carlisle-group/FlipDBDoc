# The DataTable Object

A →[*.Objects.DataTable] is a collection of named →[*.Objects.DataColumn|DataColumns] of
compatible structure. DataTables are usually obtained as the result of a query  (see the
→[*.Query.MethodList.Execute|Query.Execute] method), but they may also be constructed from
scratch. For example,  we can take three columns, each with the same number of items:

~~~
      A='S1,S2,S3'
      B='Smith,Jones,Blake'
      C=20 10 30
      A B C
 ┌───────┐  ┌───────┐  ┌────┐
 ↓S1     │  ↓Smith  │  ↓20  │
 │S2     │  │Jones  │  │10  │
 │S3     │  │Blake  │  │30  │
 └Char(2)┘  └Char(5)┘  └Int8┘
~~~

And turn them into a table by naming them:

~~~
      T=('SNO' A) ('SNAME' B)('STATUS' C)
      T
────────────────────────────────
 ┌SNO────┐  ┌SNAME──┐  ┌STATUS┐
 ↓S1     │  ↓Smith  │  ↓20    │
 │S2     │  │Jones  │  │10    │
 │S3     │  │Blake  │  │30    │
 └Char(2)┘  └Char(5)┘  └Int8──┘
── 3 rows by 3 columns ─────────
~~~

DataTables have properties and methods which may be used to inquire about and manipulate them. The
RowCount and ColumnCount properties report the number of rows and columns respectively:

~~~
      T.RowCount
┌────┐
│3   │
└Int8┘
      T.ColumnCount
┌────┐
│3   │
└Int8┘
~~~

The →[*.DataTable.Properties.ColumnNames] property reports the names of the columns in the DataTable:

~~~
      T.ColumnNames
┌───────┐
↓SNO    │
│SNAME  │
│STATUS │
└Char(6)┘
~~~

The →[*.DataTable.Properties.Columns] property returns an array of the columns in the DataTable:

~~~
      T.Columns
 ┌───────┐  ┌───────┐  ┌────┐
 ↓S1     │  ↓Smith  │  ↓20  │
 │S2     │  │Jones  │  │10  │
 │S3     │  │Blake  │  │30  │
 └Char(2)┘  └Char(5)┘  └Int8┘
~~~

Individual columns may be extracted using the →[*.DataTable.MethodList.GetColumn] method:

~~~
      T.GetColumn 'STATUS'
┌────┐
↓20  │
│10  │
│30  │
└Int8┘
~~~

Note that once a column is extracted from a DataTable, it has no name, nor any relationship to the
DataTable; it is simply data.

Individual cell values may be extracted using the →[*.DataTable.MethodList.GetItem] method,
which takes a row index and a column name as its argument:

~~~
      T.GetItem 2 'SNAME'
┌───────┐
│Blake  │
└Char(5)┘
~~~

Both the GetColumn and GetItem methods may use an index to specify the column rather than the
column name. For example:

~~~
      T.GetItem 2 1
┌───────┐
│Blake  │
└Char(5)┘
~~~

Note that all references to row or column indices are in index-origin zero. In other words, the
first row is row 0, and the second row is row 1. The →[*.DataTable.MethodList.GetRange] method
will extract a contiguous block of cells and return a DataTable. Here we start at row 1 and column
1 and extract 2 rows and 2 columns:

~~~
      T.GetRange (1 1) (2 2)
───────────────────────
 ┌───────┐  ┌────┐
 ↓Jones  │  ↓10  │
 │Blake  │  │30  │
 └Char(5)┘  └Int8┘
── 2 rows by 2 columns
~~~

The →[*.DataTable.MethodList.GetRow] method extracts a row from the table, returning a 1-row DataTable:

~~~
      T.GetRow 2
────────────────────────────────
 ┌SNO────┐  ┌SNAME──┐  ┌STATUS┐
 │S3     │  │Blake  │  │30    │
 └Char(2)┘  └Char(5)┘  └Int8──┘
── 1 rows by 3 columns ─────────
~~~

FlipDB does not have a Row or DataRow object; single rows are treated as 1-Row DataTables. It is
often necessary to iterate over the rows of a very small table, and the GetRow method provides a
means of doing so. However, it is rarely, if ever, necessary to explicitly iterate over the rows
of a large table, and it should be avoided. FlipDB functions like →[*.sum] or
→[*.firstOccurrence] provide built-in iteration that is extremely fast and efficient.

