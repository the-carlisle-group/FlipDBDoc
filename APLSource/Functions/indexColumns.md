# indexColumns

Type:{.prefix}

Table{.info}

Categories:{.prefix}

Selection{.info}

~~~
R=X indexColumns Y
~~~

X must be a DataTable or a Table, and Y must be Char or Integer. R is a DataTable formed by
selecting columns in X named or indexed in Y. The columns in R are ordered according to Y.

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
      DT indexColumns 'Three,One'
───────────────────────
 ┌Three┐  ┌One─┐
 ↓7    │  ↓1   │
 │8    │  │2   │
 │9    │  │3   │
 └Int8─┘  └Int8┘
── 3 rows by 2 columns
      DT indexColumns 2 1 0
─────────────────────────
 ┌Three┐  ┌Two─┐  ┌One─┐
 ↓7    │  ↓3   │  ↓1   │
 │8    │  │4   │  │2   │
 │9    │  │5   │  │3   │
 └Int8─┘  └Int8┘  └Int8┘
── 3 rows by 3 columns ──
~~~

