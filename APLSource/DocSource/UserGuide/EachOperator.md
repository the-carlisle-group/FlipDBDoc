# The each Operator

### Using each to Operate on Partitioned Columns

The →[*.each] operator may be used to apply a function to the items of a partitioned column,
rather than to the column as a whole. Consider the partitioned column P:

~~~
     P=enclose (1 2 3 4) (5 6) (7 8 9)
     P
┌─────────┐
↓[1,2,3,4]│
│[5,6]    │
│[7,8,9]  │
└Int8─────┘
~~~

The →[*.reverse] function may be applied directly to P, reversing its ordering:

~~~
      reverse P
┌─────────┐
↓[7,8,9]  │
│[5,6]    │
│[1,2,3,4]│
└Int8─────┘
~~~

Alternatively, the order of each item of P may be reversed:

~~~
      reverse each P
┌─────────┐
↓[4,3,2,1]│
│[6,5]    │
│[9,8,7]  │
└Int8─────┘
~~~

The each operator may also be used with dyadic functions (those with left arguments). Consider the
→[*.take] function, used without each, to extract the first 2 items of P:

~~~
      2 take P
┌─────────┐
↓[1,2,3,4]│
│[5,6]    │
└Int8─────┘
~~~

And now with each, to extract the first two values from each item of P:

~~~
      2 take each P
┌─────┐
↓[1,2]│
│[5,6]│
│[7,8]│
└Int8─┘
~~~

The each operator will apply corresponding items of the left argument to each item of the right argument:

~~~
      2 4 6 take each P
┌─────────────┐
↓[1,2]        │
│[5,6,0,0]    │
│[7,8,9,0,0,0]│
└Int8─────────┘
~~~

Note that it is not necessary to use each with aggregate functions, as by definition aggregate
functions contain an implicit each when operating on partitioned data. While the each is
redundant, applying it produces the same result:

~~~
      sum P
┌────┐
↓10  │
│11  │
│24  │
└Int8┘
      sum each P
┌────┐
↓10  │
│11  │
│24  │
└Int8┘
~~~

### Using each to Operate on Multiple Columns

The second use of each is to apply a function on or between multiple columns. Consider the columns
C1, C2, and C3:

~~~
      C1=1 2 3
      C2=4 5 6
      C3=7 8 9
      C1 C2 C3
 ┌────┐  ┌────┐  ┌────┐
 ↓1   │  ↓4   │  ↓7   │
 │2   │  │5   │  │8   │
 │3   │  │6   │  │9   │
 └Int8┘  └Int8┘  └Int8┘
~~~

The each operator may be used to apply a function to each column:

~~~
      sum each C1 C2 C3
 ┌────┐  ┌────┐  ┌────┐
 │6   │  │15  │  │24  │
 └Int8┘  └Int8┘  └Int8┘
~~~

Note that this use of the sum function to total each of three separate columns requires the each
operator, while summing each item of a single, partitioned column, as done above, does not.

A function may be applied between corresponding pairs of columns:

~~~
       C2 C3 * each C2 C3
 ┌────┐  ┌────┐
 ↓16  │  ↓49  │
 │25  │  │64  │
 │36  │  │81  │
 └Int8┘  └Int8┘
~~~

A single column is extended to match the number of columns in the other argument:

~~~
       C1 * each C2 C3
 ┌────┐  ┌────┐
 ↓ 4  │  ↓ 7  │
 │10  │  │16  │
 │18  │  │27  │
 └Int8┘  └Int8┘
~~~

### Using each to apply Functions to DataTables.

The each operator may be used to apply functions directly to a DataTable, resulting in a DataTable.
Consider the DataTable T:

~~~
       T= ('Col1' C1) ('Col2' C2) ('Col3' C3)
       T
────────────────────────
 ┌Col1┐  ┌Col2┐  ┌Col3┐
 ↓1   │  ↓4   │  ↓7   │
 │2   │  │5   │  │8   │
 │3   │  │6   │  │9   │
 └Int8┘  └Int8┘  └Int8┘
── 3 rows by 3 columns ─
~~~

Functions may be applied to the columns of a table:

~~~
     sum each T
────────────────────────
 ┌Col1┐  ┌Col2┐  ┌Col3┐
 │6   │  │15  │  │24  │
 └Int8┘  └Int8┘  └Int8┘
── 0 rows by 3 columns ─
~~~

Functions may be applied between the positionally corresponding columns of two tables:

~~~
     T + each T
────────────────────────
 ┌Col1┐  ┌Col2┐  ┌Col3┐
 ↓2   │  ↓ 8  │  ↓14  │
 │4   │  │10  │  │16  │
 │6   │  │12  │  │18  │
 └Int8┘  └Int8┘  └Int8┘
── 3 rows by 3 columns ─
~~~

Functions may be applied between a table and a set of columns:

~~~
     C1 C2 C3 * each T
────────────────────────
 ┌Col1┐  ┌Col2┐  ┌Col3┐
 ↓1   │  ↓16  │  ↓49  │
 │4   │  │25  │  │64  │
 │9   │  │36  │  │81  │
 └Int8┘  └Int8┘  └Int8┘
── 3 rows by 3 columns ─
~~~

or between a table and a single column:

~~~
     C1 * each T
────────────────────────
 ┌Col1┐  ┌Col2┐  ┌Col3┐
 ↓1   │  ↓ 4  │  ↓ 7  │
 │4   │  │10  │  │16  │
 │9   │  │18  │  │27  │
 └Int8┘  └Int8┘  └Int8┘
── 3 rows by 3 columns ─
~~~

The resulting DataTable inherits its columns names from the left argument, if it is a DataTable,
else its right argument.

