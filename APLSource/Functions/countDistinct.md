# countDistinct

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=countDistinct X
~~~

X may be any column. R is Integer being the number of unique items in X or in each partition of X.

## Example

~~~
      B1=4 4 4 4 3 3 2 2 3 1
      countDistinct B1
┌────┐
│4   │
└Int8┘
      C=enclose (2 2 3) (4 4 4 4) (1 2 3 4 4)
      C (countDistinct C)
 ┌───────────┐  ┌────┐
 ↓[2,2,3]    │  ↓2   │
 │[4,4,4,4]  │  │1   │
 │[1,2,3,4,4]│  │4   │
 └Int8───────┘  └Int8┘
~~~

