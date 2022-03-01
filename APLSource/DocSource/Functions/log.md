# log

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=[X] log Y
~~~

X and Y must both be numeric. R is numeric.

X cannot be 1 unless Y is also 1.

If X is omitted, it defaults to the numerical constant e (2.7182818...),
and the function returns the natural logarithm.

If Y is positive, R is the base X logarithm of Y. If Y is not positive, R is the lowest float
representable on the system and equal to the result of →[*.lowestFloat]. This is because the
logarithm of a non-positive number is not a real number.

When summarizing or ranging the results of a log function, it is customary to filter out invalid
results by checking where the argument is non-positive.

## Examples

~~~
      10 log 100 2
┌───────────────────┐
↓2                  │
│0.30102999566398114│
└Float──────────────┘

      2 8 10 log 4 512 10000
┌─────┐
↓2    │
│3    │
│4    │
└Float┘

      log 1 2 2.718281829 3 10
┌────────────┐
↓0           │
│0.6931471806│
│1           │
│1.098612289 │
│2.302585093 │
└Float───────┘
~~~

