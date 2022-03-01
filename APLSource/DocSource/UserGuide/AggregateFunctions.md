# Aggregate Functions

Aggregate functions summarize data and return a scalar result from a simple column. For example,
the sum function adds up the numbers in a column:

~~~
      sum 1 2 3 4 5 6
┌────┐
│21  │
└Int8┘
~~~

When applied to a partitioned column, an aggregate function operates on each item of the column
individually. Consider the following partitioned column:

~~~
      a=enclose (1 2 3) (4 5) (6 7 8 9)
      a
┌─────────┐
↓[1,2,3]  │
│[4,5]    │
│[6,7,8,9]│
└Int8─────┘
~~~

When sum is applied to this column, it adds up the numbers in each item yielding a 3 item simple column:

~~~
     sum a
┌────┐
↓6   │
│9   │
│30  │
└Int8┘
~~~

Similarly, when an aggregate function is applied to an enclosed column it operates on values inside
the item and produces a scalar result:

~~~
      b=enclose 1 2 3 4 5 6
      b
┌─────────────┐
│[1,2,3,4,5,6]│
└Int8─────────┘
      sum b
┌────┐
│21  │
└Int8┘
~~~

When applied to an empty simple column, an aggregate function still produces a scalar. The value of
this scalar is usually related to the identity function associated with the aggregate function.
Thus the sum function produces a 0 when applied to an empty column:

~~~
      sum emptyInteger
┌────┐
│0   │
└Int8┘
~~~

While the product function produces a 1 when applied to an empty column:

~~~
      product emptyInteger
┌────┐
│1   │
└Int8┘
~~~

Finally, when applied to a scalar item, an aggregate function returns its argument unchanged:

~~~
      sum 5
┌────┐
│5   │
└Int8┘
~~~

All aggregate functions work in this way. Thus, an aggregate function applied to a partitioned
column always returns a simple column of the same shape, and an aggregate function applied to a
simple column always returns a scalar column.

FlipDB provides a complete set of aggregate numeric, logical, statistical, and financial functions.

