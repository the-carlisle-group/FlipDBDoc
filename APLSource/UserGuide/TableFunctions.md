# Table Functions

In addition to properties and methods, FlipDB provides a set of functions that operate on
DataTables. While methods usually operate on DataTables in-place functions never modify their
arguments, and always return a new instance of a DataTable. For example, given the table DT:

~~~
      DT=('One' (1 2 3)) ('Two' (4 5 6)) ('Three' (7 8 9)) ('Four' (10 11 12))
      DT
─────────────────────────────────
 ┌One─┐  ┌Two─┐  ┌Three┐  ┌Four┐
 ↓1   │  ↓4   │  ↓7    │  ↓10  │
 │2   │  │5   │  │8    │  │11  │
 │3   │  │6   │  │9    │  │12  │
 └Int8┘  └Int8┘  └Int8─┘  └Int8┘
── 3 rows by 4 columns ──────────
~~~

The →[*.withoutColumns] function returns a new DataTable DT2 leaving DT unchanged:

~~~
      DT2=DT withoutColumns 'One,Four'
      DT2
───────────────────────
 ┌Two─┐  ┌Three┐
 ↓4   │  ↓7    │
 │5   │  │8    │
 │6   │  │9    │
 └Int8┘  └Int8─┘
── 3 rows by 2 columns
      DT
─────────────────────────────────
 ┌One─┐  ┌Two─┐  ┌Three┐  ┌Four┐
 ↓1   │  ↓4   │  ↓7    │  ↓10  │
 │2   │  │5   │  │8    │  │11  │
 │3   │  │6   │  │9    │  │12  │
 └Int8┘  └Int8┘  └Int8─┘  └Int8┘
── 3 rows by 4 columns ──────────
~~~

...while the →[*.DeleteColumns] method removes columns directly from DT:

~~~
      R=DT.DeleteColumns 'One,Four'
      DT
───────────────────────
 ┌Two─┐  ┌Three┐
 ↓4   │  ↓7    │
 │5   │  │8    │
 │6   │  │9    │
 └Int8┘  └Int8─┘
── 3 rows by 2 columns
~~~

