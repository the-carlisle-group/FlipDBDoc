# nTile

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Partitions a column into groups. {.purpose}

~~~
R=X nTile Y
~~~

Y is any column. X is a postive integer. R is Integer from 0 to X-1.
R partitions Y into X equal sized groups, if possible. If the length of Y is not a perfect
a perfect multiple of X, the groups at the beginning will be 1 larger as necessary.

## Examples
~~~
      A=integers 10
      A (2 nTile A) (3 nTile A) (4 nTile A)
 ┌────┐  ┌────┐  ┌────┐  ┌────┐
 ↓0   │  ↓0   │  ↓0   │  ↓0   │
 │1   │  │0   │  │0   │  │0   │
 │2   │  │0   │  │0   │  │0   │
 │3   │  │0   │  │0   │  │1   │
 │4   │  │0   │  │1   │  │1   │
 │5   │  │1   │  │1   │  │1   │
 │6   │  │1   │  │1   │  │2   │
 │7   │  │1   │  │2   │  │2   │
 │8   │  │1   │  │2   │  │3   │
 │9   │  │1   │  │2   │  │3   │
 └Int8┘  └Int8┘  └Int8┘  └Int8┘
~~~
