# Hybrid functions

Hybrid functions are a cross between scalar and aggregate functions and exhibit qualities of both.
Like a scalar function, hybrid functions produce a result that corresponds in structure to its
argument(s), but like an aggregate function, the result for a particular item will depend on other
items or the ordering of the items in the column.

Consider the hybrid function →[*.plusScan] function which computes the running sum of a column:

~~~
      plusScan 1 2 3 4 5
┌────┐
↓1   │
│3   │
│6   │
│10  │
│15  │
└Int8┘
~~~

Applying the function to the same 5 values, but in a different order, produces a different result:

~~~
      plusScan 5 4 3 2 1
┌────┐
↓5   │
│9   │
│12  │
│14  │
│15  │
└Int8┘
~~~

Like aggregate functions, hybrid functions operate on each item of a partitioned column independently:

~~~
      plusScan enclose (1 2 3) (3 2 1) (1 1 2 3 5 8)
┌───────────────┐
↓[1,3,6]        │
│[3,5,6]        │
│[1,2,4,7,12,20]│
└Int8───────────┘
~~~

Other hybrid functions include sortUp:

~~~
      sortUp 'Phil,Paul,Adam,Steve'
┌───────┐
↓Adam   │
│Paul   │
│Phil   │
│Steve  │
└Char(5)┘
~~~

and sortDown:

~~~
      sortDown 'Phil,Paul,Adam,Steve'
┌───────┐
↓Steve  │
│Phil   │
│Paul   │
│Adam   │
└Char(5)┘
~~~

There is a hybrid function for flagging the first occurrence of each unique value in a column:

~~~
      firstOccurrence 1 2 1 2 3 1 2 3 4
┌───────┐
↓1      │
│1      │
│0      │
│0      │
│1      │
│0      │
│0      │
│0      │
│1      │
└Boolean┘
~~~

There are hybrid functions for all of the useful boolean scans. For example, the andScan function,
which is a running logical and, turns off all but the leading 1's:

~~~
       andScan 1 1 1 0 0 1 1
┌───────┐
↓1      │
│1      │
│1      │
│0      │
│0      │
│0      │
│0      │
└Boolean┘
~~~

