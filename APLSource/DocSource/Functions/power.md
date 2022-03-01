# power

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=[X] power Y
~~~

X and Y must be numeric. R is X raised to the exponent Y.  R is numeric.
If X is omitted, it defaults to the numerical contant e (2.7182818...)
and power is the exponential function.

## Examples

~~~
      2 power 2 -2
┌─────┐
↓4    │
│0.25 │
└Float┘
      9 64 power 0.5
┌─────┐
↓3    │
│8    │
└Float┘

      power 0 1 2 3 4 5
┌─────────────┐
↓  1          │
│  2.718281828│
│  7.389056099│
│ 20.08553692 │
│ 54.59815003 │
│148.4131591  │
└Float────────┘

      -27 power 2 3
┌──────┐
↓   729│
│-19683│
└Int16─┘
      -27 power 1 2 divide 3
┌───────────────────┐
↓-2.9999999999999996│
│ 8.999999999999998 │
└Float──────────────┘
      -27 power 1.2
┌────────────────┐
│52.1959152131576│
└Float───────────┘
~~~

