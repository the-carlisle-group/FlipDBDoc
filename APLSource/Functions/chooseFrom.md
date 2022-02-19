# chooseFrom

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

chooseFrom.{.purpose}

~~~
R=X chooseFrom Y
~~~

X and Y must both be numeric. R is numeric.

For positive integer arguments, R is the number of selections of X things from Y things:

~~~
      3 chooseFrom 5
┌────┐
│10  │
└Int8┘
~~~

If Y is a negative integer then X must be a whole number (integer). An element of R is integer if
corresponding elements of X and Y are integers. chooseFrom is defined in terms of the function
Factorial for positive integer arguments:

~~~
     X chooseFrom Y
~~~

is equivalent to:

~~~
      (factorial Y) times (reciprocal (factorial X) times (factorial Y minus X))
~~~

## Examples

~~~
      1 1.2 1.4 1.6 1.8 2 chooseFrom 5
┌──────────────────┐
↓ 5                │
│ 6.105689247785092│
│ 7.219424685598466│
│ 8.281104786421768│
│ 9.227916704038833│
│10                │
└Float─────────────┘
~~~

