# names

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Returns the names in a PropertySpace, or the column names of a table.{.purpose}

~~~
R=names X
~~~

If X is a PropertySpace, R is list of names in X.
If X is a Table or DataTable, R is list of column names in X.

See also: →[values]

## Examples

~~~
      P=newPropertySpace 'C1,C2,C3' ((1 2 3) (4 5 6 ) (7 8 9))
      names P
┌───────┐
↓C1     │
│C2     │
│C3     │
└Char(2)┘
      transpose P
────────────────────────
 ┌C1──┐  ┌C2──┐  ┌C3──┐
 ↓1   │  ↓4   │  ↓7   │
 │2   │  │5   │  │8   │
 │3   │  │6   │  │9   │
 └Int8┘  └Int8┘  └Int8┘
── 3 rows by 3 columns ─
      names transpose P
┌───────┐
↓C1     │
│C2     │
│C3     │
└Char(2)┘
~~~

