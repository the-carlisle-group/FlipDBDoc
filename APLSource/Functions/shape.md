# shape

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Arithmetic{.info}

Returns the number of elements in its argument.{.purpose}

~~~
R=shape X
~~~

X may be any column or object array. R is a scalar Integer being the number of items in X.

If X is scalar or enclosed, R is 1. If X is Simple, R is the number of elements. If X is
partitioned, R is the number of partitions. If X is an object array, R is the number of objects.

See also: →[count], →[reshape]

## Examples

~~~
      shape 'Hello world'
┌────┐
│1   │
└Int8┘
      shape 4 5 6
┌────┐
│3   │
└Int8┘
      shape 'A,B,C,D'
┌────┐
│4   │
└Int8┘
      shape ('hello world') (1 2 3 4) ('A,B,C')
┌────┐
│3   │
└Int8┘
~~~

