# whereColumns

Type:{.prefix}

Table{.info}

Categories:{.prefix}

Selection{.info}

~~~
R=X whereColumns Y
~~~

X must be a DataTable or a Table, and Y must be Boolean. The length of Y must correspond to the
number of columns in X. R is a DataTable formed by selecting columns in X where Y is true.

See also: →[withColumns], →[withoutColumns]

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
      DT whereColumns 1 0 1
───────────────────────
 ┌One─┐  ┌Three┐
 ↓1   │  ↓7    │
 │2   │  │8    │
 │3   │  │9    │
 └Int8┘  └Int8─┘
── 3 rows by 2 columns
~~~

