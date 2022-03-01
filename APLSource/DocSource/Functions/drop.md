# drop

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Selection{.info}

Remove a leading or trailing portion.{.purpose}

~~~
R=X drop Y
~~~

X is a scalar Integer. Y is any column or object array.

R is Y with X elements, partitions or objects removed from the start if X is positive or the end if
X is negative.

If absolute of X is greater than or equal to the shape of Y, the result is empty.

See also: →[take], →[dropChar], →[takeChar]

## Examples:

~~~
      a='one,two,three'
      b=1 2 3
      a b
 ┌───────┐  ┌────┐
 ↓one    │  ↓1   │
 │two    │  │2   │
 │three  │  │3   │
 └Char(5)┘  └Int8┘
      1 drop a
┌───────┐
↓two    │
│three  │
└Char(5)┘
      -2 drop b
┌────┐
↓1   │
└Int8┘
      8 drop a
┌───────┐
↓Char(5)┘
~~~

