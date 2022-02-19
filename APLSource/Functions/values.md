# values

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Returns the values in a PropertySpace, or the column values of a table.{.purpose}

~~~
R=values X
~~~

If X is a PropertySpace, R is an array of values associated with the names in X.

If X is Table or DataTable, R is an array of DataColumns.

See also: →[names]

## Examples

~~~
      P=newPropertySpace 'C1,C2,C3' ((1 2 3) (4 5 6 ) (7 8 9))
      values P
 ┌────┐  ┌────┐  ┌────┐
 ↓1   │  ↓4   │  ↓7   │
 │2   │  │5   │  │8   │
 │3   │  │6   │  │9   │
 └Int8┘  └Int8┘  └Int8┘
      transpose P
────────────────────────
 ┌C1──┐  ┌C2──┐  ┌C3──┐
 ↓1   │  ↓4   │  ↓7   │
 │2   │  │5   │  │8   │
 │3   │  │6   │  │9   │
 └Int8┘  └Int8┘  └Int8┘
── 3 rows by 3 columns ─
      values transpose P
 ┌────┐  ┌────┐  ┌────┐
 ↓1   │  ↓4   │  ↓7   │
 │2   │  │5   │  │8   │
 │3   │  │6   │  │9   │
 └Int8┘  └Int8┘  └Int8
~~~

