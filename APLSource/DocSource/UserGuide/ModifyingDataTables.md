# Modifying DataTables

The →[*.DataTable.MethodList.AddColumn] method will tack on a new column to an existing table:

~~~
      T.AddColumn 'CITY' 'London,Paris,Paris'
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME──┐  ┌STATUS┐  ┌CITY───┐
 ↓S1     │  ↓Smith  │  ↓20    │  ↓London │
 │S2     │  │Jones  │  │10    │  │Paris  │
 │S3     │  │Blake  │  │30    │  │Paris  │
 └Char(2)┘  └Char(5)┘  └Int8──┘  └Char(6)┘
── 3 rows by 4 columns ────────────────────
~~~

The AddColumn method requires the column to conform to the table, so it must a scalar or enclosed,
or have the same number of items as there are rows as the table.

The →[*.DataTable.MethodList.AddRows] method adds rows to the table:

~~~
      T.AddRows 'S4,S5' 'Clarke,Adams' (20 30) ('Lindon,Athens')
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME──┐  ┌STATUS┐  ┌CITY───┐
 ↓S1     │  ↓Smith  │  ↓20    │  ↓London │
 │S2     │  │Jones  │  │10    │  │Paris  │
 │S3     │  │Blake  │  │30    │  │Paris  │
 │S4     │  │Clarke │  │20    │  │Lindon │
 │S5     │  │Adams  │  │30    │  │Athens │
 └Char(2)┘  └Char(6)┘  └Int8──┘  └Char(6)┘
── 5 rows by 4 columns ────────────────────
~~~

In this case, when the AddRows methods takes an array of columns as its argument, all columns must
be provided, and they must be provided in the proper order.

The →[*.DataTable.MethodList.SetItem] method pokes a value into a specific cell:

~~~
      T.SetItem 3 'CITY' 'London'
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME──┐  ┌STATUS┐  ┌CITY───┐
 ↓S1     │  ↓Smith  │  ↓20    │  ↓London │
 │S2     │  │Jones  │  │10    │  │Paris  │
 │S3     │  │Blake  │  │30    │  │Paris  │
 │S4     │  │Clarke │  │20    │  │London │
 │S5     │  │Adams  │  │30    │  │Athens │
 └Char(2)┘  └Char(6)┘  └Int8──┘  └Char(6)┘
── 5 rows by 4 columns ────────────────────
~~~

Note that each of these methods changes the DataTable in place. The result of the method is a
reference to the same instance. There are other methods that return new instances rather than
modifying a table in place.

