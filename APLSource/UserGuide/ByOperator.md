# The by Operator

As we have seen, each element in the result of a hybrid function depends on all of the elements of
the argument column, as well as the ordering of those elements. Consider the following columns.
First, an arbitrary id column, just for reference:

~~~
      id=1 2 3 4 5
~~~

Then a corresponding column of values, these could be sales numbers, or payments, for example:

~~~
      v=10 5 7 20 2
~~~

And finally an associated timeseries column that orders the values:

~~~
      t="2012-01-01,2011-12-31,2012-01-15,2012-01-10,2012-01-5"
~~~

Let's review these columns:

~~~
      id v t
 ┌────┐  ┌────┐  ┌──────────┐
 ↓1   │  ↓10  │  ↓2012-01-01│
 │2   │  │5   │  │2011-12-31│
 │3   │  │7   │  │2012-01-15│
 │4   │  │20  │  │2012-01-10│
 │5   │  │2   │  │2012-01-05│
 └Int8┘  └Int8┘  └Date──────┘
~~~

Now let's apply the hybrid function runningSum, which computes a running sum, to the column v:

~~~
      runningSum v
┌────┐
↓10  │
│15  │
│22  │
│42  │
│44  │
└Int8┘
~~~

By inspection, we see that the for id number 4, the result is 42, which is the sum of 10, 5, 7 and
20. Now let's say we want to order the values by the timeseries column t, and then compute the
running sum. This will clearly give a different result. Furthermore, we want the final result to
be back in the original order. All of this can simply and easily be done using the by operator
in conjunction with the order function. First, we use the order function to create a column of
indices that will be used to sort the values:

~~~
      i=order t 'Asc'
      i
┌────┐
↓1   │
│0   │
│4   │
│3   │
│2   │
└Int8┘
~~~

Now we insert the by operator between the function and its argument, and tack on the indexing column:

~~~
      runningSum by v i
┌────┐
↓15  │
│5   │
│44  │
│37  │
│17  │
└Int8┘
~~~

Let's review this result adjacent to the inputs:

~~~
      id v t  (runningSum by v i)
 ┌────┐  ┌────┐  ┌──────────┐  ┌────┐
 ↓1   │  ↓10  │  ↓2012-01-01│  ↓15  │
 │2   │  │5   │  │2011-12-31│  │5   │
 │3   │  │7   │  │2012-01-15│  │44  │
 │4   │  │20  │  │2012-01-10│  │37  │
 │5   │  │2   │  │2012-01-05│  │17  │
 └Int8┘  └Int8┘  └Date──────┘  └Int8┘
~~~

Here we see that the result for id number 4 is now 37. If we inspect the timeseries column t, we
see that the associated date is January 10th, and that every date except January 15th, which is
associated with id number 3, precedes it. The sum of all the values on and preceding January 10th
is the sum of everything except 7, which is in fact 37.

Thus we see that the by operator orders the argument by the indexing column, applies the hybrid
function, and then puts the result back in the original order. This can be explicitly seen be
simulating the operator using the →gradeUp and →index functions:

~~~
      (gradeUp gradeUp t) index runningSum (gradeUp t) index v
┌────┐
↓15  │
│5   │
│44  │
│37  │
│17  │
└Int8┘
~~~

Now consider a classification column that divides our data set into two groups, A and B:

~~~
      c='A,B,A,B,B'
      id c v t
 ┌────┐  ┌───────┐  ┌────┐  ┌──────────┐
 ↓1   │  ↓A      │  ↓10  │  ↓2012-01-01│
 │2   │  │B      │  │5   │  │2011-12-31│
 │3   │  │A      │  │7   │  │2012-01-15│
 │4   │  │B      │  │20  │  │2012-01-10│
 │5   │  │B      │  │2   │  │2012-01-05│
 └Int8┘  └Char(1)┘  └Int8┘  └Date──────┘
~~~

As we have noted, hybrid functions depend not only the order of the elements, but also on the total
number of elements in the argument. It is often useful to apply a hybrid function to distinct
groups within a column, based on the unique values of another corresponding column. The by
operator does this in conjunction with the group function. First we apply the group function to
classification column (or columns), creating a partitioned indexing column:

~~~
      g=group c
      g
┌───────┐
↓[0,2]  │
│[1,3,4]│
└Int8───┘
~~~

Then we use the by operator to apply the function to each group:

~~~
      runningSum by v g
┌────┐
↓10  │
│5   │
│17  │
│25  │
│27  │
└Int8┘
~~~

Note that the values here are lower than above, as the running sum is computed separately for the
A's and the B's rather than for the entire column. For example, for id number 5, the result is 27,
which is the sum of only the values for group B: 5, 20 and 2.

The group function takes an optional left argument, which is a sorting directive, so we can not
only apply the hybrid function to different groups, but in a specific order as well:

~~~
      runningSum by v (t 'Asc'  group c)
┌────┐
↓10  │
│5   │
│17  │
│27  │
│7   │
└Int8┘
~~~

Note again that the by operator returns a result that is in the original order of the data.

Generally, the by operator is used only in conjunction with hybrid functions. Scalar function may
not be used with groupBy. However, it is sometimes useful to use the by operator with aggregate
functions. Consider the aggregate function sum applied to v grouped by c:

~~~
      sum by v (group c)
┌────┐
↓17  │
│27  │
│17  │
│27  │
│27  │
└Int8┘
~~~

By inspection we see that the sum of v for group A is 17, while for B it is 27. However, the result
is not two elements, but rather 5, which corresponds to the shape of the input v. When an
aggregate function is used with the by operator and a partitioned indexing column the function is
applied to each group, and the results are replicated to correspond to the original value and
classification columns. Thus, for example, we can compute the percentage that each value
contributes to its group:

~~~
      100 * v / sum by v (group c)
┌────────────────┐
↓58.823529411765 │
│18.518518518519 │
│41.176470588235 │
│74.074074074074 │
│ 7.4074074074074│
└Float───────────┘
~~~

