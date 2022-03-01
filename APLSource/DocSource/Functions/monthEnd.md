# monthEnd

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

~~~
R=monthEnd X
~~~

X is Date or DateTime. R is same type as X, but with the day set to the last day the month, and the
time, if applicable, set to 0.

## Examples

~~~
     monthEnd '2009-02-14,2000-01-01'
┌──────────┐
↓2009-02-28│
│2000-01-31│
└Date──────┘
      monthEnd currentDateTime
┌───────────────────┐
│2010-01-31 00:00:00│
└DateTime───────────┘
     monthEnd emptyDate
┌──────────┐
↓Date──────┘
~~~

