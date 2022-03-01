# Structure

A DataColumn has one of four possible structures: scalar, simple, enclosed, and partitioned.
(These structures are sometimes noted numerically as 0, 1, 2 and 3 respectively.) A scalar column
is a single, atomic unit of data:

~~~
      5
┌────┐
│5   │
└Int8┘
~~~

A single character string is also a scalar:

~~~
      'E.F. Codd'
┌─────────┐
│E.F. Codd│
└Char(9)──┘
~~~

A simple column is an ordered list of scalar items:

~~~
      3 1 5 8 1
┌────┐
↓3   │
│1   │
│5   │
│8   │
│1   │
└Int8┘
~~~

A Simple character column may be entered as a comma delimited string:

~~~
      'E.F. Codd,C. J. Date,K.E. Iverson'
┌────────────┐
↓E.F. Codd   │
│C. J. Date  │
│K.E. Iverson│
└Char(12)────┘
~~~

Similarly, a list of dates may be entered as a comma delimited string:

~~~
      '1776/07/04,1941/12/07,2002/01/01'
┌──────────┐
↓1776-07-04│
│1941-12-07│
│2002-01-01│
└Date──────┘
~~~

Simple columns may have 0 or more items in them. Because entering a single item returns a scalar,
we must use the enlist function to create 1-item simple columns:

~~~
      enlist 5
┌────┐
↓5   │
└Int8┘
~~~

To create empty simple columns we use specially provided functions. The emptyInteger function
returns a simple integer columns with zero items:

~~~
      emptyInteger
┌────┐
↓Int8┘
~~~

There are similar functions for all of the other data types:

~~~
      emptyBoolean emptyChar emptyDate
 ┌───────┐  ┌───────┐  ┌────┐
 ↓Boolean┘  ↓Char(0)┘  ↓Date┘
~~~

Note the down arrow in the upper left corner of the display box that indicates the row dimension of
simple columns that does not exist for scalar columns:

~~~
      5  (enlist 5) 'Codd' (enlist 'Codd')
 ┌────┐  ┌────┐  ┌───────┐  ┌───────┐
 │5   │  ↓5   │  │Codd   │  ↓Codd   │
 └Int8┘  └Int8┘  └Char(4)┘  └Char(4)┘
~~~

Enclosed columns contain a single, non-atomic item, which itself is a simple column. The
→[##.##.Reference.Functions.enclose] function converts a simple column to an enclosed column:

~~~
      enclose 3 1 5 8 1
┌───────────┐
│[3,1,5,8,1]│
└Int8───────┘
~~~

and the →[##.##.Reference.Functions.disclose] function performs the inverse:

~~~
      disclose enclose 3 1 5 8 1
┌────┐
↓3   │
│1   │
│5   │
│8   │
│1   │
└Int8┘
~~~

Partitioned columns contain an array of enclosed columns. The enclose function is also used to
construct a partitioned column:

~~~
      enclose (1 2 3) (4 5) (6 7 8 9)
┌─────────┐
↓[1,2,3]  │
│[4,5]    │
│[6,7,8,9]│
└Int8─────┘
~~~

Note that both simple columns and partitioned columns have a row dimension, while scalar and
enclosed columns do not.

