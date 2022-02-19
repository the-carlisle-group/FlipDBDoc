# SaveChanges Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method saves the pending changes in a DataTable to the Table. In addition, schema enhancements
may be made to the Table in order for it to receive the changes representation in the DataTable.

The argument is composed of 1 item:

|-|-|
|1|DataTable|DataTable|

The result is composed of 1 item:

|-|-|
|1|Dummy|0|

Consider the suppliers table from the suppliers and parts database:

~~~
      D=Scripting.Server.OpenDatabase 'SandP'
      T=D.GetTable 'S'
      DT=T.ExecuteQuery ''
      DT
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌SNAME───┐  ┌STATUS┐  ┌CITY────┐
 ↓S1     │  ↓Smith   │  ↓20    │  ↓London  │
 │S2     │  │Jones   │  │10    │  │Paris   │
 │S3     │  │Blake   │  │30    │  │Paris   │
 │S4     │  │Clark   │  │20    │  │London  │
 │S5     │  │Adams   │  │30    │  │Athens  │
 └Char(2)┘  └Char(10)┘  └Int8──┘  └Char(10)┘
── 5 rows by 4 columns ──────────────────────
~~~

In this case, the DataTable DT is an in-memory represenation of the entire suppliers table. (If a
Where or Select clause had been applied in the query, then the DataTable would represent a subset
of the table.) Changes can be made to the DataTable:

~~~
      J=DT.AddRows 'S6' 'Codd' 40 'San Francisco'
      J=DT.SetItem 1 3 'Rome'
      DT
── Key:SNO ───────────────────────────────────────
 ┌SNO────┐  ┌SNAME───┐  ┌STATUS┐  ┌CITY─────────┐
 ↓S1     │  ↓Smith   │  ↓20    │  ↓London       │
 │S2     │  │Jones   │  │10    │  │Rome         │
 │S3     │  │Blake   │  │30    │  │Paris        │
 │S4     │  │Clark   │  │20    │  │London       │
 │S5     │  │Adams   │  │30    │  │Athens       │
 │S6     │  │Codd    │  │40    │  │San Francisco│
 └Char(2)┘  └Char(10)┘  └Int8──┘  └Char(13)─────┘
── 6 rows by 4 columns ───────────────────────────
~~~

And these changes can be saved back to the Table:

~~~
       T.SaveChanges DT
┌───────┐
│0      │
└Boolean┘
      T
── SandP.S ───────────────────────────────────────
 ┌SNO────┐  ┌SNAME───┐  ┌STATUS┐  ┌CITY─────────┐
 ↓S1     │  ↓Smith   │  ↓20    │  ↓London       │
 │S3     │  │Blake   │  │30    │  │Paris        │
 │S4     │  │Clark   │  │20    │  │London       │
 │S5     │  │Adams   │  │30    │  │Athens       │
 │S6     │  │Codd    │  │40    │  │San Francisco│
 │S2     │  │Jones   │  │10    │  │Rome         │
 └Char(2)┘  └Char(10)┘  └Int8──┘  └Char(13)─────┘
── 6 rows by 4 columns ───────────────────────────
~~~

