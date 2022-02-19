# sum

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Arithmetic{.info}

Sum; Total{.purpose}

~~~
R=sum X
~~~

X must be numeric.  R is the sum of X.  R is numeric.

## Examples

~~~
      sum 1 2 3 4 5 6 7 8 9 10
┌────┐
│55  │
└Int8┘
      sum 5
┌────┐
│5   │
└Int8┘
      sum emptyInteger
┌────┐
│0   │
└Int8┘
      a=enclose (1 2 3) (1 2) (4 5 6 7)
      sum a
┌────┐
↓6   │
│3   │
│22  │
└Int8┘
~~~

