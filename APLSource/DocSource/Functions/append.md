# append

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Extend one column with the items of another.{.purpose}

~~~
R=X append Y
~~~

X and Y are columns of compatible types or one or both are object arrays. R is composed of the
items of X followed by the items of Y whether elements, partitions or objects.

If either X or Y is an array, then R is an array. Otherwise if either X or Y is enclosed or
partitioned, then R is partitioned. Otherwise R is simple.

If one but not both of X & Y is enclosed or partitioned each element of the other is enclosed to
form a partition of one item.

The type of R is determined such that no precision is lost.

## Examples

~~~
      a='one,two'
      b=1 2
      'zero' append a
┌───────┐
↓zero   │
│one    │
│two    │
└Char(4)┘
      b append 3
┌────┐
↓1   │
│2   │
│3   │
└Int8┘
      a b append a b
 ┌───────┐  ┌────┐  ┌───────┐  ┌────┐
 ↓one    │  ↓1   │  ↓one    │  ↓1   │
 │two    │  │2   │  │two    │  │2   │
 └Char(3)┘  └Int8┘  └Char(3)┘  └Int8┘
~~~

