# transpose

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Rearranges the items of a partitioned column,
or converts a DataTable to a PropertySpace and vice-versa.{.purpose}

~~~
R=[Y] transpose X
~~~

If X is a single partitioned column, R is X regrouped such that
the first element of each item of X is grouped together, the second
element of each item of X is grouped, etc.

If all partitions are of equal length, this is effectively transposing
a matrix, and transposing a second time will result in the original data column;
in this case transpose is a self-inverse function.

If the partitions of X are different lenghts, the function is not a self-inverse,
except in the special case where the items are sorted from longest to shortest
partition.

If X is a DataTable and Y omitted or 0, R is a PropertySpace, while if X is a PropertySpace,
R is a DataTable. In this case transpose is a self-inverse function. Note
that transposing a PropertySpace only works if the values are all
DataColumns and have the same length (or are scalar). A PropertySpace
is a more flexible data structure than a DataTable, and thus not every
PropertySpace may be transposed to a DataTable.

Y is 1, and X is a DataTable where all simple columns are of compatible type,
then R is a DataTable with two columns, Name and Value, where Name is composed
of the column names and Y, and Value is a partioned column composed of all the
columns in Y. The function is a self inverse in this case.

## Examples

Transposing a single partitioned column:
~~~
       A=enclose (1 2 3 4) (5) (6 7)
       A
┌─────────┐
↓[1,2,3,4]│
│[5]      │
│[6,7]    │
└Int8─────┘
      transpose A
┌───────┐
↓[1,5,6]│
│[2,7]  │
│[3]    │
│[4]    │
└Int8───┘
      transpose enclose (1 2 3 4) (5 6 7 8) (9 10 11 12)
┌────────┐
↓[1,5,9] │
│[2,6,10]│
│[3,7,11]│
│[4,8,12]│
└Int8────┘
      transpose transpose enclose (1 2 3 4) (5 6 7 8) (9 10 11 12)
┌────────────┐
↓[1,2,3,4]   │
│[5,6,7,8]   │
│[9,10,11,12]│
└Int8────────┘

~~~

Transposing DataTables and PropertySpaces:

~~~
      P=newPropertySpace 0
      P.Name='Bob,Paul,Steve'
      P.Balance=25000 35000 20000
      P.Rate=3.125 4 2.875
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
 ↓25,000 │  ↓Bob    │  ↓3.125 │
 │35,000 │  │Paul   │  │4.000 │
 │20,000 │  │Steve  │  │2.875 │
 └Int32──┘  └Char(5)┘  └Dec(3)┘
── 3 rows by 3 columns ─────────
      transpose T
┌PropertySpace───────────┐
│ Name         Type      │
│ ---------    --------- │
│ Balance      Integer   │
│ Name         Char      │
│ Rate         Decimal   │
└────────────────────────┘
      P.Name
┌Name───┐
↓Bob    │
│Paul   │
│Steve  │
└Char(5)┘
      T.GetColumn 'Name'
┌───────┐
↓Bob    │
│Paul   │
│Steve  │
└Char(5)┘
~~~

Transposing a DataTable to a Name/Value DataTable and back:

~~~
      P=newPropertySpace 0
      P.ColOne=1 2 3 4 5
      P.ColTwo=6 7 8 9 10
      P.ColThree=11 12 13 14 15
      T=transpose P
      T
────────────────────────────────
 ┌ColOne┐  ┌ColThree┐  ┌ColTwo┐
 ↓1     │  ↓11      │  ↓6     │
 │2     │  │12      │  │7     │
 │3     │  │13      │  │8     │
 │4     │  │14      │  │9     │
 │5     │  │15      │  │10    │
 └Int8──┘  └Int8────┘  └Int8──┘
── 5 rows by 3 columns ─────────
      1 transpose T
────────────────────────────────
 ┌Name────┐  ┌Value───────────┐
 ↓ColOne  │  ↓[1,2,3,4,5]     │
 │ColThree│  │[11,12,13,14,15]│
 │ColTwo  │  │[6,7,8,9,10]    │
 └Char(8)─┘  └Int8────────────┘
── 3 rows by 2 columns ─────────
      1 transpose 1 transpose T
────────────────────────────────
 ┌ColOne┐  ┌ColThree┐  ┌ColTwo┐
 ↓1     │  ↓11      │  ↓6     │
 │2     │  │12      │  │7     │
 │3     │  │13      │  │8     │
 │4     │  │14      │  │9     │
 │5     │  │15      │  │10    │
 └Int8──┘  └Int8────┘  └Int8──┘
── 5 rows by 3 columns ─────────
~~~

