# number

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Misc{.info}

~~~
R=number X
~~~

X is any column. R is the integers from 0 to N-1, where N is the count of X.
R is the same shape and structure as X.

This function is equivalent to SQLs ROW_NUMBER function.

This function is equivalent to:

~~~
integers count X
~~~

See also: →[rankDown] →[rankUp] →[percentileRank]

Use the →[group] and →[order] functions with the →[*.by] operator to number rows within groups
and in a specific order.

## Examples
~~~
      A= 85 80 85 72 71 99 72 85 98 89
      C='X,X,Y,Y,Y,X,X,Y,X,Y'

      A C (number A) (number by A (group C))
 ┌────┐  ┌───────┐  ┌────┐  ┌────┐
 ↓85  │  ↓X      │  ↓0   │  ↓0   │
 │80  │  │X      │  │1   │  │1   │
 │85  │  │Y      │  │2   │  │0   │
 │72  │  │Y      │  │3   │  │1   │
 │71  │  │Y      │  │4   │  │2   │
 │99  │  │X      │  │5   │  │2   │
 │72  │  │X      │  │6   │  │3   │
 │85  │  │Y      │  │7   │  │3   │
 │98  │  │X      │  │8   │  │4   │
 │89  │  │Y      │  │9   │  │4   │
 └Int8┘  └Char(1)┘  └Int8┘  └Int8┘
~~~
