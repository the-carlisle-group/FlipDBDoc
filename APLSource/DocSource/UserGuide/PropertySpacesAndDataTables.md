# PropertySpaces and DataTables

If all of the values in a PropertySpace are DataColumns, and these columns
all have the same length, then the PropertySpace
may be converted to a DataTable using the transpose function.
This provides a convenient and alternative syntax for defining
DataTables:
~~~
      P=newPropertySpace ''
      P.Name='Paul,Bob,Steve'
      P.Balance=100000 125000 85000
      P.Rate=3.5 5.125 4.875
      P
┌PropertySpace───────────┐
│ Name         Type      │
│ ---------    --------- │
│ Balance      Integer   │
│ Name         Char      │
│ Rate         Decimal   │
└────────────────────────┘
      T=transpose P
      T
────────────────────────────────
 ┌Balance┐  ┌Name───┐  ┌Rate──┐
 ↓100,000│  ↓Paul   │  ↓3.500 │
 │125,000│  │Bob    │  │5.125 │
 │85,000 │  │Steve  │  │4.875 │
 └Int32──┘  └Char(5)┘  └Dec(3)┘
── 3 rows by 3 columns ─────────
~~~
The transpose function is a self-inverse, so a DataTable may be converted
back to a PropertySpace:
~~~
      transpose T
┌PropertySpace───────────┐
│ Name         Type      │
│ ---------    --------- │
│ Balance      Integer   │
│ Name         Char      │
│ Rate         Decimal   │
└────────────────────────┘
~~~
Note that while every DataTable may be converted to a PropertySpace, not
every propertySpace may be converted to a DataTable.

## Ordering
The names in a PropertySpace are not formally ordered. However, FlipDB
attempts to order them for presentation purposes.
When creating and populating a PropertySpace in the session or in a script,
the default ordering is alphabetical. When a PropertySpace is created
from a DataTable, the ordering of the columns in the DataTable is preserved.
If the primary purpose of an ExpressionSet is creating and populating a
PropertySpace, FlipDB attempts to sort the names in the order of assignment.


Names may explicitly ordered using the →sort function:
~~~

~~~
Certain commonly used sets of names are ordered in certain ways if no explicit
ordering is defined. For example, creating an expression set 'Name'

