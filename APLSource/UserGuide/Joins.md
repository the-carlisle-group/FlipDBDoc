# Joins

In the relational model, data is often spread among many tables. One of the primary advantages of
this is that redundant or duplicated data is eliminated, and the database is much more likely to
remain consistent.

However, a seeming disadvantage of this is that when writing a query the data that we need is often
in a table other than what we might consider our starting table or primary table. In traditional
SQL, the solution is to "join" these two tables, combining them to create a larger, temporary
table, and then to apply a where and select clause to this temporary table. In FlipDB, however, we
can take a different approach, and simply and directly specify a column from another table that we
would like to appear in our starting table.

One profound advantage of this technique is when we need to access data from more than two tables
for a single query. In SQL, this will require a multi-table join that can be both difficult to
write and expensive to execute. In FlipDB we can directly specify the columns we need whether they
come from one table or a dozen tables.

In FlipDB there are 5 different join syntaxes. Their formal definitions follow below, and the
remainder of this chapter covers each syntax in detail.

## 1. Simple Join

~~~
      FK.COL
~~~

A compound name consisting of two column names separated by a dot. The first name, preceding the
dot, is assumed to be a foreign key in the starting table T. The second name, following the dot,
is assumed to be a column in the table T2 referenced by the foreign key. This implies a
many-to-one relationship, and the data in COL is replicated and ordered to correspond to the rows
of T. The type of FK.COL is inherited from COL. The structure of FK.COL is simple.

## 2. Partitioned Join

~~~
      T2[FK].COL
~~~

This is a compound name consisting of a table name and a bracketed column name, followed by a dot
and another column name. FK is assumed to be a foreign key column in table T2 that references the
starting table T. The name following the dot, COL, is assumed to be a column in table T2. This
implies a one-to-many relationship, and the data in COL is partitioned to correspond with the rows
of T. The type of T2[FK].COL is inherited from COL. The structure is partitioned.

## 3. Enclosed Join

~~~
      T2[].COL
~~~

A compound name consisting of table with empty brackets, followed by a dot and a column name. The
table T2 refers to any table, and COL is assumed to be column in T2. In this case there is no
specified relationship between the starting table and table T2, so the entire column COL is
enclosed. The result type is inherited from COL. The result structure is enclosed.

#### 4. Time Series Join

~~~
      T2[X].COL
~~~

A compound name consisting of table name and a bracketed value X followed by a dot and a column
name. The table T2 refers to a time series table. A time series table is simply a table with a
foreign key pointing back to T1, and a Date column named "AsOfDate". This foreign key and the
AsOfDate should together form an alternate key. COL is assumed to be a column in the time series
table T2. X may be an explicit date, an integer, or one of the values "min", "max" "first" or
"last". If X is an explicit date, the result is the value of COL for that date. The key word "max"
represents a specific date: the maximum date found in the AsOfDate column. The key word "min"
represents a specific date: the minimum date found in the AsOfDate column. The key word "last"
represents the most recent AsOfDate for each item. The key word "first" represents the earliest
AsOfDate for each item. A negative integer -N indicates N observations before the last value of
AsOfDate. A positive integer P indicates P observations after the first value of AsOfDate. Zero is
the same as "last" The result type is inherited from COL. The result structure is simple.

## 5. General Join

~~~
      T2[L1;L2].COL
~~~

A compound name consisting of a table name and then, between brackets, two lists of column names,
followed by a dot and a column name. L1 represents a lookup key, and is a list of zero or more
column names in the starting table T1. L2 is a list, the same length as L1, of column names in T2.
The types of the columns named in L1 must be compatible with the columns named in L2. The names of
the corresponding columns in each key need not match. If, however, the names and ordering in L2
are identical to L1, then L2 may be elided, but the semicolon must remain. COL is a column in
table T2. The data in COL is replicated, re-ordered and possibly partitioned to correspond to the
rows of the starting table T1. The result type is inherited from COL. The result structure is
simple or partitioned, depending on the relationship implied by the lookup key.

## A Note on Terminology

In the following sections, when two tables are under discussion, and one table has a foreign key
referring to the other table, the one with the foreign key is often called the "long" table, while
the table that the foreign key refers to is called the "short" table. This is just shorthand for
noting the many-to-one relationship implied by the foreign key, and that the "many" table will
usually be longer than the "one" table. Of course, in practice the "many" table may have few or no
rows in it and thus actually be shorter.

