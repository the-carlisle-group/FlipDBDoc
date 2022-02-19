# ExecuteQuery Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

The argument may be void or composed of 2 items:

|-|-|
|1|WhereClause|String|
|2|SelectClause|String|

The result is composed of 1 item:

|-|-|
|1|Result|DataTable|

This method executes simple queries that require at most a simple where and select clause. For
complex queries use the →[*.Objects.Query] object. For example:

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

