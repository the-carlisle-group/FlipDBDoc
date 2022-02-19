# defaultColumn

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Misc{.info}

~~~
R=default X Y
~~~

X is the name of a column, Y is an expression. X must be Char, and Y may be of any type. R is the
value of column named X if the column exists, else Y.

Note that defaultColumn is a special function that requires an implicit table, and thus only works
in the context of a query.  It will not work in the session or in a script.

## Examples

~~~
      S=Scripting.Server
      T=S.Get '/Databases/SandP/P'
      Q=T.Query ''
      J=Q.AddColumn 'PartNumber' 'PNO'
      J=Q.AddColumn 'Weight' 'defaultColumn "WEIGHT" 5'
      J=Q.AddColumn 'Weight3' 'defaultColumn "WEIGHT3" (WEIGHT * 3)'
      Q.Execute 0
───────────────────────────────────
 ┌PartNumber┐  ┌Weight┐  ┌Weight3┐
 ↓P1        │  ↓12    │  ↓36     │
 │P2        │  │17    │  │51     │
 │P3        │  │17    │  │51     │
 │P4        │  │14    │  │42     │
 │P5        │  │12    │  │36     │
 │P6        │  │19    │  │57     │
 └Char(2)───┘  └Int8──┘  └Int8───┘
── 6 rows by 3 columns ────────────
~~~

