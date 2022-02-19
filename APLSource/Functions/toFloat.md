# toFloat

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=toFloat X
~~~

Converts column X to type Float. X may be any type except Blob. R is 0 where X does not convert to
a valid number.

Note that any lines which contain digits are likely to produce a number, because all non-digits but
these: `.-¯` are removed.

## Examples

~~~
      toFloat '0,1,1.123,-4.5'
┌──────┐
↓ 0    │
│ 1    │
│ 1.123│
│-4.5  │
└Float─┘
      toFloat '2016-12-31'
┌────────┐
│20161231│
└Float───┘
~~~

