# whereRows

Type:{.prefix}

Table{.info}

Categories:{.prefix}

Selection{.info}

~~~
R=X whereRows Y
~~~

X must be a DataTable or a Table, and Y must be Boolean. The length of Y must correspond to the
number of rows in X. R is a DataTable formed by selecting rows in X if where Y is true.

See also: →[indexRows]

## Examples:

~~~
       DT=('One' (1 2 3)) ('Two' (3 4 5)) ('Three' (7 8 9))
       DT
─────────────────────────
 ┌One─┐  ┌Two─┐  ┌Three┐
 ↓1   │  ↓3   │  ↓7    │
 │2   │  │4   │  │8    │
 │3   │  │5   │  │9    │
 └Int8┘  └Int8┘  └Int8─┘
── 3 rows by 3 columns ──
       DT whereRows 1 0 1
─────────────────────────
 ┌One─┐  ┌Two─┐  ┌Three┐
 ↓1   │  ↓3   │  ↓7    │
 │3   │  │5   │  │9    │
 └Int8┘  └Int8┘  └Int8─┘
── 2 rows by 3 columns ──
~~~

