# reshape

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

~~~
R=X reshape Y
~~~

X is a non-negative Integer. Y is any column or object array. R is X elements, partitions or
objects of Y repeated cyclically if X is greater than →[shape] Y; except that if Y is an empty
array X must be zero.

(shape R) eq X

## Examples:

~~~
      a='one,two,three'
      b= 9 8 7
      2 reshape a
┌───────┐
↓one    │
│two    │
└Char(5)┘
      4 reshape b
┌────┐
↓9   │
│8   │
│7   │
│9   │
└Int8┘
      3 reshape a b
 ┌───────┐  ┌────┐  ┌───────┐
 ↓one    │  ↓9   │  ↓one    │
 │two    │  │8   │  │two    │
 │three  │  │7   │  │three  │
 └Char(5)┘  └Int8┘  └Char(5)┘
~~~

