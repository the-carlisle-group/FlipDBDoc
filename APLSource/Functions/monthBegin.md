# monthBegin

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

~~~
R=monthBegin X
~~~

X is Date or DateTime. R is same type as X, but with the day set to the first of the month, and the
time, if applicable, set to 0.

## Examples

~~~
     monthBegin '2009-02-14,2000-01-01'
┌──────────┐
↓2009-02-01│
│2000-01-01│
└Date──────┘
      monthBegin currentDateTime
┌───────────────────┐
│2010-01-01 00:00:00│
└DateTime───────────┘
     monthBegin emptyDate
┌──────────┐
↓Date──────┘
~~~

