# index

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Selection{.info}

Select one or more items{.purpose}

~~~
R=X forgivingIndex Y
~~~

X is a scalar or simple Integer column. Y is any column. R is composed of selected
items, whether elements or partitions or objects of Y as indexed by the elements of X. The →[shape]
of R is the shape of X.

Zero (0) indexes the first or only item with subsequent items being indexed by increasingly
positive integers.

Negative one (-1) indexes the final item with preceding items being indexed by increasingly
negative integers.

Indices that are out of range by exactly 1 (ie, where X=N, N being `→[shape] Y`) return the
prototype for the data type of Y.

Indices that are out of range by 2 or more (X gt N), generate a "Column index error".

See also: →[index], →[pick]

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

