# index

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Selection{.info}

Select one or more items{.purpose}

~~~
R=X index Y
~~~

If X is a scalar or simple Integer column,
Y may be any column or object array.
R is composed of selected items, whether elements,
partitions or objects of Y as indexed by the elements of X. The →[shape]
of R is the shape of X.

If X is a partitioned Integer column, Y must be simple, and R is Y
partitioned, selected and sorted according to X.

Zero (0) indexes the first or only item with subsequent items being indexed by increasingly
positive integers.

Negative one (-1) indexes the final item with preceding items being indexed by increasingly
negative integers.

Out of range indices, (X lt -N) or (X ge N) where N is (shape Y), generate a "Column index error".

See also: →[pick]

## Examples:

~~~
      t='zero,one,two,three,four'
      n=10 11 12 13 14
      0 -1 index t
┌───────┐
↓zero   │
│four   │
└Char(5)┘
      -2 2 2 index n
┌────┐
↓13  │
│12  │
│12  │
└Int8┘
      2 index t n t
┌───────┐
↓zero   │
│one    │
│two    │
│three  │
│four   │
└Char(5)┘
~~~

