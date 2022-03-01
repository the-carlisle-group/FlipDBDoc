# withoutColumns

Type:{.prefix}

Table{.info}

Categories:{.prefix}

Selection{.info}

~~~
R=X withoutColumns Y
~~~

X must be a DataTable or a Table, and Y must be Char. R is a DataTable formed by selecting columns
in X if they are not named in Y.

## Examples:

~~~
      DT=('One' (1 2 3)) ('Two' (4 5 6)) ('Three' (7 8 9))
      DT
─────────────────────────
 ┌One─┐  ┌Two─┐  ┌Three┐
 ↓1   │  ↓4   │  ↓7    │
 │2   │  │5   │  │8    │
 │3   │  │6   │  │9    │
 └Int8┘  └Int8┘  └Int8─┘
── 3 rows by 3 columns ──
      DT.withoutColumns 'Two'
───────────────────────
 ┌One─┐  ┌Three┐
 ↓1   │  ↓7    │
 │2   │  │8    │
 │3   │  │9    │
 └Int8┘  └Int8─┘
── 3 rows by 2 columns
~~~

Note that columns in the result are returned in the order of the columns in the source table,
regardless of the ordering of the names in the right argument. Note also that names in the right
argument that do not exist in the table are ignored:

~~~
      DT.withoutColumns 'Three,One,Five'
───────────────────────
 ┌Two─┐
 ↓4   │
 │5   │
 │6   │
 └Int8┘
── 3 rows by 1 columns
~~~

