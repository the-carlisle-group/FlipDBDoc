# nonZeroLow

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

lower, lesser, non-zero{.purpose}

~~~
R=X nonZeroLow Y
~~~

X and Y must both be numeric. R is the lower value of X and Y unless the lower is zero when the
other value is taken. R is numeric.

See also: →[minimum], →[min]

## Examples

~~~
      0 10 20 nonZeroLow 1 2 3
┌────┐
↓1   │
│2   │
│3   │
└Int8┘
~~~

