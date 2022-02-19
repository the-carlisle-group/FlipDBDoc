# indexRows

Type:{.prefix}

Table{.info}

Categories:{.prefix}

Selection{.info}

~~~
R=X indexRows Y
~~~

X must be a DataTable or a Table, and Y must be integer. R is a DataTable formed by selecting the
rows in X specified by the indices in Y. The index-origin is zero. Negative indices indicate rows
from the bottom of the table.

## Examples:

~~~
      DT=('One' (1 2 3 4 5)) ('Two' ('A,B,C,D,E'))
      DT
───────────────────────
 ┌One─┐  ┌Two────┐
 ↓1   │  ↓A      │
 │2   │  │B      │
 │3   │  │C      │
 │4   │  │D      │
 │5   │  │E      │
 └Int8┘  └Char(1)┘
── 5 rows by 2 columns
      DT indexRows 0 2 4
───────────────────────
 ┌One─┐  ┌Two────┐
 ↓1   │  ↓A      │
 │3   │  │C      │
 │5   │  │E      │
 └Int8┘  └Char(1)┘
── 3 rows by 2 columns
      DT indexRows 0 -1
───────────────────────
 ┌One─┐  ┌Two────┐
 ↓1   │  ↓A      │
 │5   │  │E      │
 └Int8┘  └Char(1)┘
── 2 rows by 2 columns
~~~

