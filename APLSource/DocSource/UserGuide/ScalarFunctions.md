# Scalar Functions

Scalar functions operate independently on the scalar items of their arguments. They may be dyadic,
taking a left and right argument, or monadic, taking a right argument only. The basic arithmetic
functions plus, minus, times, and divide are dyadic scalar functions. For example, adding two
scalars produces a scalar:

~~~
      2+3
┌────┐
│5   │
└Int8┘
~~~

While adding two simple columns, produces a simple column:

~~~
      1 2 3 + 4 5 6
┌────┐
↓5   │
│7   │
│9   │
└Int8┘
~~~

Here the plus function is applied to each of the corresponding scalar items in its arguments, and
the result are the same shape as each of the arguments. In this case the shape of the arguments is
3, and thus the result has 3 items as well.

In the context of a database table, the results of a scalar function are said to be row-independent
and do not depend on the number or order of rows. If a column has n rows, then any scalar function
applied to that column will result in a column with n rows. Furthermore the result for the ith row
depends only on the ith row of the argument.

Each of the arguments must have the same number of items, or an error will result:

~~~
      1 2 + 4 5 6
Column length error.
~~~

However, if one of the arguments is a scalar, it will be extended to match the shape of the other
argument. Thus:

~~~
      10 + 4 5 6
┌────┐
↓14  │
│15  │
│16  │
└Int8┘
~~~

Is equivalent to:

~~~
      10 10 10 + 4 5 6
┌────┐
↓14  │
│15  │
│16  │
└Int8┘
~~~

Scalar functions pervade enclosed and partitioned columns. For example, we can add 10 to every
scalar item in an enclosed column:

~~~
     10 + enclose 1 2 3
┌──────────┐
│[11,12,13]│
└Int8──────┘
~~~

Or consider a couple of partitioned columns:

~~~
      a=enlist (1 2 3) (4 5 6 7) (8 9)
      b=enlist (10 11 12) (13 14 15 16) (17 18)
      a b
 ┌─────────┐  ┌─────────────┐
 ↓[1,2,3]  │  ↓[10,11,12]   │
 │[4,5,6,7]│  │[13,14,15,16]│
 │[8,9]    │  │[17,18]      │
 └Int8─────┘  └Int8─────────┘
~~~

Because the corresponding items of each of these columns is the same length, they may be added together:

~~~
      a + b
┌─────────────┐
↓[11,13,15]   │
│[17,19,21,23]│
│[25,27]      │
└Int8─────────┘
~~~

Scalar extension works with partitioned columns as well, so we can add 10 to every item in the
column a:

~~~
      10+a
┌─────────────┐
↓[11,12,13]   │
│[14,15,16,17]│
│[18,19]      │
└Int8─────────┘
~~~

Scalar extension works at every level, so that we may add a simple column to a partitioned column,
as long as they both have the same number of items. In this case, each scalar in the simple column
is extended to match the shape of each item in the partitioned column:

~~~
      a + 100 200 300
┌─────────────────┐
↓[101,102,103]    │
│[204,205,206,207]│
│[308,309]        │
└Int16────────────┘
~~~

Finally, consider adding an enclosed column to a partitioned column:

~~~
      a + enclose 100 200 300
Column length error.
~~~

Here we get an error, because the enclosed column on the right has 3 items, but not all of the
items in the column on the left are of length 3. We can add an enclosed column to a partitioned
column if the shape of each item in the partitioned column is the same as the shape of the item in
the enclosed column. To see this, let's defined a new partitioned column where each item has the
same shape:

~~~
      c=enlist (1 2 3 4) (5 6 7 8) (9 10 11 12)
      c
┌────────────┐
↓[1,2,3,4]   │
│[5,6,7,8]   │
│[9,10,11,12]│
└Int8────────┘
~~~

Now we can add an enclosed column whose item is also of shape 4:

~~~
      c + enclose 100 200 300 400
┌─────────────────┐
↓[101,202,303,404]│
│[105,206,307,408]│
│[109,210,311,412]│
└Int16────────────┘
~~~

All scalar functions work in this same manner. FlipDB provides a wide range of scalar functions in
addition to  the primary arithmetic functions. There are logical, relational, statistical and
financial functions, as well as functions for casting data types, for string manipulation, and for
performing date and time calculations.

