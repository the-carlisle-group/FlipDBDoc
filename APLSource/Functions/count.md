# count

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=count X
~~~

X may be any column. R is Integer being the number of items in X or each partition of X.

If X is Scalar, R is 1. If X is Simple, R is the number of items. If X is Enclosed, R is the number
of items in the enclosure. If X is Partitioned, R is the number of items in each partition.

For the total number of scalar items in any column whatever its structure:

~~~
sum count X
~~~

See also: →[shape]

## Examples

~~~
      B1=1 2 3 4 5
      count B1
┌────┐
│5   │
└Int8┘
      count 1
┌────┐
│1   │
└Int8┘
      count emptyArray
┌────┐
│0   │
└Int8┘
      C=enclose (1 2 3) (1 2) (1 2 3 4 5)
      C  (count C)
 ┌───────────┐  ┌────┐
 ↓[1,2,3]    │  ↓3   │
 │[1,2]      │  │2   │
 │[1,2,3,4,5]│  │5   │
 └Int8───────┘  └Int8┘
~~~

