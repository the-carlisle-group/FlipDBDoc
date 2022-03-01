# deal

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Produce a set of "random" numbers.{.purpose}

~~~
R=X deal Y
~~~

X and Y are both Integer scalars. R is X numbers selected "randomly" from the first Y non-negative
integers and is simple. X must be non-negative and less than or equal to Y.

## Examples:

~~~
      (1 deal 5)(2 deal 5)(3 deal 5)(4 deal 5)(5 deal 5)
 ┌────┐  ┌────┐  ┌────┐  ┌────┐  ┌────┐
 ↓2   │  ↓1   │  ↓4   │  ↓2   │  ↓2   │
 └Int8┘  │4   │  │0   │  │1   │  │4   │
         └Int8┘  │2   │  │4   │  │1   │
                 └Int8┘  │0   │  │3   │
                         └Int8┘  │0   │
                                 └Int8┘
~~~

