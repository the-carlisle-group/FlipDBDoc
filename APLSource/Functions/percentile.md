# percentile

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical{.info}

~~~
R=X percentile Y
~~~

R is Numeric and in the range of X.

This function is equivalent to the Excel percentile and percentile.inc function when there is an
odd number of values in Y. For even numbers it agrees with 'Mann' pg.111.

## Examples

~~~
     25 percentile 87 74 89 94 91 63 98
┌────┐
│74  │
└Int8┘
~~~

