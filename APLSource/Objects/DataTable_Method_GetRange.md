# GetRange Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

This method returns a contiguous range of data from the DataTable.

The argument is composed of 2 items:

|-|-|
|1|Position|2-item integer|
|2|Size|2 item integer|

The result is composed of 1 item:

|-|-|
|1|Result|DataTable|

The first item of the argument is the index of the upper left corner of the range, in index-origin
zero.  The second item of the argument specifies the number of rows and columns in the range. For example:

~~~
       D=Scripting.Server.OpenDatabase 'SandP'
       T=D.GetTable 'S'
       DT=T.ExecuteQuery ''
       DT
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐  ┌SNAME───┐
 ↓S1     │  ↓20    │  ↓London  │  ↓Smith   │
 │S2     │  │10    │  │Paris   │  │Jones   │
 │S3     │  │30    │  │Paris   │  │Blake   │
 │S4     │  │20    │  │London  │  │Clark   │
 │S5     │  │30    │  │Athens  │  │Adams   │
 └Char(2)┘  └Int8──┘  └Char(10)┘  └Char(10)┘
── 5 rows by 4 columns ──────────────────────
      DT.GetRange (2 2)(3 2)
────────────────────────
 ┌────────┐  ┌────────┐
 ↓Paris   │  ↓Blake   │
 │London  │  │Clark   │
 │Athens  │  │Adams   │
 └Char(10)┘  └Char(10)┘
── 3 rows by 2 columns ─
~~~

