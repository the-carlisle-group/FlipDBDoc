# DataTables and Keys

The →[*.DataTable.Properties.Key] property of a →[*.Objects.DataTable] specifies the name of one
or more columns. and is an implicit argument referenced by any DataTable method that requires a
row lookup between two DataTables, including →[*.DataTable.MethodList.AddRows],
→[*.DataTable.MethodList.UpdateRows], and →[*.DataTable.MethodList.AddColumns].

Consider the following two DataTables:

~~~
      T1=('SNO' 'S1,S2,S3') ('STATUS' (20 10 30))
      T1
───────────────────────
 ┌SNO────┐  ┌STATUS┐
 ↓S1     │  ↓20    │
 │S2     │  │10    │
 │S3     │  │30    │
 └Char(2)┘  └Int8──┘
── 3 rows by 2 columns
      T2=('SNO' 'S3,S1,S4') ('STATUS' (50 60 70)) ('SNAME' 'Blake,Smith,Clark')
      T2
────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌SNAME──┐
 ↓S3     │  ↓50    │  ↓Blake  │
 │S1     │  │60    │  │Smith  │
 │S4     │  │70    │  │Clark  │
 └Char(2)┘  └Int8──┘  └Char(5)┘
── 3 rows by 3 columns ─────────
~~~

Now specify the key property for each table, and then apply the AddRows method to add all rows to
T1 that are in T2 that do not already appear in T1, namely supplier S4:

~~~
      T1.Key='SNO'
      T2.Key='SNO'
      T1.AddRows T2
── Key:SNO ────────────
 ┌SNO────┐  ┌STATUS┐
 ↓S1     │  ↓20    │
 │S2     │  │10    │
 │S3     │  │30    │
 │S4     │  │70    │
 └Char(2)┘  └Int8──┘
── 4 rows by 2 columns
~~~

The cells in T1 may be updated with the appropriate values of T2, with the UpdateRows method:

~~~
      T1.UpdateRows T2
── Key:SNO ────────────
 ┌SNO────┐  ┌STATUS┐
 ↓S1     │  ↓60    │
 │S2     │  │10    │
 │S3     │  │50    │
 │S4     │  │70    │
 └Char(2)┘  └Int8──┘
── 4 rows by 2 columns
~~~

The UpdateRows method will update a cell in the instance table (in the case table T1), if, in the
argument table (in this case T2) it finds a corresponding row according to the Key property, and
also a column of the same name and compatible type.

Columns in T2 that do not exist in T1 may be added to T1 with the AddColumns method:

~~~
      T1.AddColumns T2
── Key:SNO ─────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌SNAME──┐
 ↓S1     │  ↓60    │  ↓Smith  │
 │S2     │  │10    │  │       │
 │S3     │  │50    │  │Blake  │
 │S4     │  │70    │  │Clark  │
 └Char(2)┘  └Int8──┘  └Char(5)┘
── 4 rows by 3 columns ─────────
~~~

The Key property of a DataTable is distinct from the primary key, alternate key, and foreign key
concepts of a Table object, though it may represent them.

A DataTable does **not** enforce uniqueness on a column named as the key column, as the column may or
may not represent a primary key, and uniqueness may not be appropriate.

When setting the Key property, the columns specified must exist. However, when referencing the Key
property,  the columns specified may or may not exist. Thus methods that implicitly use the Key
property, like AddRows, may signal a "Key column not found" error.

If either one or both tables have no key specified, then no rows are considered to match.
Otherwise, the key columns specified for each table must correspond in number and type, or an
error will be signalled. The actual names of the key columns do not need to match, so they must be
specified in the proper order. For example the Key 'SNO,PNO' is **not** the same as 'PNO,SNO'.

The default value of the Key property of a new instance of a DataTable is empty. However, if the
DataTable is the result of a database query and the query result includes the primary key column
of the starting table, then the Key property defaults to the name of the primary key column.

The UpdateRows method updates the cells in one table with the values from another table based on
matching rows as determined by the Key property:

~~~
      T.Key='SNO'
      T2=('SNO' 'S1,S2')('STATUS'(99 0))
      T2.Key='SNO'
      T.UpdateRows T2
─  Key:SNO ────────────────────────────────
 ┌SNO────┐  ┌SNAME──┐  ┌STATUS┐  ┌CITY───┐
 ↓S1     │  ↓Smith  │  ↓99    │  ↓London │
 │S2     │  │Jones  │  │ 0    │  │Paris  │
 │S3     │  │Blake  │  │30    │  │Paris  │
 │S4     │  │Clarke │  │20    │  │London │
 │S5     │  │Adams  │  │30    │  │Athens │
 └Char(2)┘  └Char(6)┘  └Int8──┘  └Char(6)┘
── 5 rows by 4 columns ────────────────────
~~~

The UpdateRows method changes the DataTable in place. Other methods that implicitly reference the
Key property are AddRows and AddColumns.

