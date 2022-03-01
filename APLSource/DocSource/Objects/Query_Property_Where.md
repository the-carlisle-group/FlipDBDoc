# Where Property

Applies to:{.prefix}

→[##.##.Query]{.info}

The Where property specifies one or more Boolean expressions (statement) used to filter the number
of rows in the starting table. It is analogous to the Where clause in a SQL statement.

The Where property is formally a DataTable with an Expression column (a Boolean expression set),
and may be specified as such, but it may also conveniently be specified as a simple string:

~~~
      Q.Where='COLOR eq "Red"'
      Q.Where
───────────────────────
 ┌Expression────┐
 ↓COLOR eq 'Red'│
 └Char(14)──────┘
── 1 rows by 1 columns
~~~

The helper method →[##.MethodList.AddWhere] may be used to incrementally add a where clause:

~~~
      Q.AddWhere 'WEIGHT ge 14'
───────────────────────
 ┌Expression────┐
 ↓COLOR eq 'Red'│
 │WEIGHT ge 14  │
 └Char(14)──────┘
── 2 rows by 1 columns
~~~

Regardless of how the Where property is specified, it always returns a DataTable.

If multiple statements are provided, they are logically "and-ed" together. Thus the above is
equivalent to:

~~~
      Q.Where='(COLOR eq "Red") and (WEIGHT ge 14)'
      Q.Where
───────────────────────────────────────
 ┌Expression─────────────────────────┐
 ↓(COLOR eq 'Red') and (WEIGHT ge 14)│
 └Char(35)───────────────────────────┘
── 1 rows by 1 columns ────────────────
~~~

Note that for readability, debugging, and performance, it is better to use many short statements
rather than few long statements.

Where statements are processed sequentially. If none of the statements are row-dependent, then the
statement order is not relevant in terms of the final selected rows. However, for performance, the
most restrictive statements should be placed first, eliminating as many rows as soon as possible.

If one or more statements is row-dependent, then order may be critical. Consider the suppliers table:

~~~
      S=Server
      S=Scripting.Server
      T=S.Get '/Databases/SandP/S'
      T
── SandP.S ──────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐  ┌SNAME───┐
 ↓S1     │  ↓20    │  ↓London  │  ↓Smith   │
 │S2     │  │10    │  │Paris   │  │Jones   │
 │S3     │  │30    │  │Paris   │  │Blake   │
 │S4     │  │20    │  │London  │  │Clark   │
 │S5     │  │30    │  │Athens  │  │Adams   │
 └Char(2)┘  └Int8──┘  └Char(10)┘  └Char(10)┘
── 5 rows by 4 columns ──────────────────────
~~~

And then the following query:

~~~
      Q=T.Query ''
      J=Q.AddWhere 'firstOccurrence CITY'
      J=Q.AddWhere 'STATUS in 30'
      Q.Execute 0
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐  ┌SNAME───┐
 ↓S5     │  ↓30    │  ↓Athens  │  ↓Adams   │
 └Char(2)┘  └Int8──┘  └Char(10)┘  └Char(10)┘
── 1 rows by 4 columns ──────────────────────
~~~

And now the same query but with the where statements re-ordered:

~~~
      Q=T.Query ''
      J=Q.AddWhere 'STATUS in 30'
      J=Q.AddWhere 'firstOccurrence CITY'
      Q.Execute 0
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐  ┌SNAME───┐
 ↓S3     │  ↓30    │  ↓Paris   │  ↓Blake   │
 │S5     │  │30    │  │Athens  │  │Adams   │
 └Char(2)┘  └Int8──┘  └Char(10)┘  └Char(10)┘
── 2 rows by 4 columns ──────────────────────
~~~

