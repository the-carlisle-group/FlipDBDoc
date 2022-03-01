# min

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Arithmetic{.info}

minimum, lowest{.purpose}

~~~
R=min X
~~~

X must be numeric. R is the lowest value of X. For an empty set, R is the highest possible number
that may be represented on the system.

See also: →[minimum], →[nonZeroLow], →[max]

## Examples

~~~
      min 7 3 19 4
┌────┐
│3   │
└Int8┘
      min 1 0
┌────┐
│0   │
└Int8┘
      min emptyInt
┌──────────────────────┐
│1.7976931348623157E308│
└Int32─────────────────┘
~~~

