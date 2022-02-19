# max

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Arithmetic{.info}

maximum, highest{.purpose}

~~~
R=max X
~~~

X must be numeric. R is the highest value of X. For an empty set, R is the lowest possible number
that may be represented on the system.

See also: →[maximum], →[min]

## Examples

~~~
      max 8 5 3 19
┌────┐
│19  │
└Int8┘
       max 1 0
┌───────┐
│1      │
└Boolean┘
     max emptyInt
┌───────────────────────┐
│-1.7976931348623157E308│
└Int32──────────────────┘
~~~

