# ExecuteQuery Method

Applies to:{.prefix}

→[##.##.Table]{.info}

The argument may be void or composed of 2 items:

|-|-|
|1|WhereClause|String|
|2|SelectClause|String|

The result is composed of 1 item:

|-|-|
|1|Result|DataTable|

This method executes simple queries that require at most a simple where and select clause. For
complex queries use the Query object. For example:

~~~
      D=Scripting.Server.OpenDatabase 'SandP'
      T=D.GetTable 'S'
      T.ExecuteQuery 'CITY in "London"' 'SNO,SNAME'
── Key:SNO ────────────
 ┌SNO────┐  ┌SNAME───┐
 ↓S1     │  ↓Smith   │
 │S4     │  │Clark   │
 └Char(2)┘  └Char(10)┘
── 2 rows by 2 columns
~~~

This method provides the easiest way to materialize an entire Table as a DataTable, by calling it
with an empty or void argument:

~~~
      T.ExecuteQuery ''
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐  ┌SNAME───┐
 ↓S1     │  ↓20    │  ↓London  │  ↓Smith   │
 │S2     │  │10    │  │Paris   │  │Jones   │
 │S3     │  │30    │  │Paris   │  │Blake   │
 │S4     │  │20    │  │London  │  │Clark   │
 │S5     │  │30    │  │Athens  │  │Adams   │
 └Char(2)┘  └Int8──┘  └Char(10)┘  └Char(10)┘
── 5 rows by 4 columns ──────────────────────
~~~

