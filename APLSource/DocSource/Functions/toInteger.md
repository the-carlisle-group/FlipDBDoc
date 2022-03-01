# toInteger

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=toInteger X
~~~

Convert column X to type Integer. X may be any column type except Blob. If X is Float or Decimal, R
is the truncated value of X. If X is Date or DateTime R is an 8 digit integer of the form
ccyymmdd, or 0. If X is Char, R is 0 if X does not convert to a valid number.

Note that any lines which contain digits are likely to produce a number, because all non-digits but
these: `.-¯` are removed.

## Examples

~~~
      toInteger '1,3.14,NAN,36 inches,4x7'
┌────┐
↓1   │
│3   │
│0   │
│36  │
│0   │
└Int8┘
      toInteger '2016-12-31'
┌──────────┐
│20,161,231│
└Int32─────┘
~~~

