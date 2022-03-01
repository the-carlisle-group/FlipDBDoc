# The across Operator

The across operator is used to apply an aggregate function "across" a set of columns rather than
"down" a single column. Consider the aggregate function sum. Like all aggregate functions, sum is
applied down a column. So, creating a simple column of integers:

~~~
      a=1 2 3 4 5 6 7
      a
┌────┐
↓1   │
│2   │
│3   │
│4   │
│5   │
│6   │
│7   │
└Int8┘
~~~

We can apply sum and get the total of the items in the column:

~~~
      sum a
┌────┐
│28  │
└Int8┘
~~~

Furthermore, like all aggregate functions, sum operates on each item of a partitioned column. Let's
create a partitioned column:

~~~
      p=enclose (1 2 3) (5 6 7 8 9) (10 11)
      p
┌───────────┐
↓[1,2,3]    │
│[5,6,7,8,9]│
│[10,11]    │
└Int8───────┘
~~~

And then apply the sum function:

~~~
      sum p
┌────┐
↓6   │
│35  │
│21  │
└Int8┘
~~~

Now consider two more columns of integers, along with our original column:

~~~
      b=8 9 10 11 12 13 14
      c=15 16 17 18 19 20 21
      a b c
 ┌────┐  ┌────┐  ┌────┐
 ↓1   │  ↓8   │  ↓15  │
 │2   │  │9   │  │16  │
 │3   │  │10  │  │17  │
 │4   │  │11  │  │18  │
 │5   │  │12  │  │19  │
 │6   │  │13  │  │20  │
 │7   │  │14  │  │21  │
 └Int8┘  └Int8┘  └Int8┘
~~~

What are some of the ways we may apply the sum function to these three columns simultaneously? If
we try to apply the sum function directly to the three columns we get an error:

~~~
      sum a b c
Column domain error.
~~~

This is because sum requires a single column as its argument. The each operator, as we have seen,
may be used to apply the sum function to each column individually:

~~~
      sum each a b c
 ┌────┐  ┌────┐  ┌────┐
 │28  │  │77  │  │126 │
 └Int8┘  └Int8┘  └Int8┘
~~~

and is equivalent to calling the sum function three separate times:

~~~
      (sum a) (sum b) (sum c)
 ┌────┐  ┌────┐  ┌────┐
 │28  │  │77  │  │126 │
 └Int8┘  └Int8┘  └Int8┘
~~~

With the each operator, we are still summing down each column. The across operator, however, allows
us to sum across the rows rather than down the columns:

~~~
      sum across a b c
┌────┐
↓24  │
│27  │
│30  │
│33  │
│36  │
│39  │
│42  │
└Int8┘
~~~

The across operator may be thought of as converting an aggregate function into a scalar function
that returns a value for each row in a table. The across operator works by rearranging the
multiple columns into a single, partitioned column using the combine function, and then applying
the aggregate function. We can see how the transpose function works on the three columns above:

~~~
      combine a b c
┌─────────┐
↓[1,8,15] │
│[2,9,16] │
│[3,10,17]│
│[4,11,18]│
│[5,12,19]│
│[6,13,20]│
│[7,14,21]│
└Int8─────┘
~~~

The combine function has combined the corresponding items in the argument columns into a single
item in the resulting partitioned column. If we apply sum to this partitioned result, we get the
exact same result as using sum with the across operator on the separate columns:

~~~
      sum combine a b c
┌────┐
↓24  │
│27  │
│30  │
│33  │
│36  │
│39  │
│42  │
└Int8┘
~~~

The across operator works with all aggregate functions. The coalesce function is commonly used with across:

~~~
       coalesce across (0 0 1) (5 0 2) (7 8 9)
┌────┐
↓5   │
│8   │
│1   │
└Int8┘
~~~

There are a few aggregate functions, like weightedAverage and correlation, that take multiple
arguments. When using the across operator with these functions, each argument should be placed
within parenthesis. To see how this works, let's create another column, so we now have four
corresponding columns:

~~~
      d=22 23 24 25 26 27 28
      a b c d
 ┌────┐  ┌────┐  ┌────┐  ┌────┐
 ↓1   │  ↓8   │  ↓15  │  ↓22  │
 │2   │  │9   │  │16  │  │23  │
 │3   │  │10  │  │17  │  │24  │
 │4   │  │11  │  │18  │  │25  │
 │5   │  │12  │  │19  │  │26  │
 │6   │  │13  │  │20  │  │27  │
 │7   │  │14  │  │21  │  │28  │
 └Int8┘  └Int8┘  └Int8┘  └Int8┘
~~~

Now, to compute, say, the weighted average across columns a and b, weighted respectively by columns
c and d, we execute the function on the transpose of each a and b, and c and d:

~~~
      weightedAverage across (a b) (c d)
┌────────────────┐
↓ 5.1621621621622│
│ 6.1282051282051│
│ 7.0975609756098│
│ 8.0697674418605│
│ 9.0444444444444│
│10.021276595745 │
│11              │
└Float───────────┘
~~~

