# weekday

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

~~~
R=weekday X
~~~

X must be Date. R is the day of the week on which each date in X falls. R is Char having the same
structure as X.

## Examples

~~~
      weekday '2012-11-6'
┌───────┐
│Tuesday│
└Char(9)┘
      weekday '2013-12-25,,2013-12-31'
┌─────────┐
↓Wednesday│
│         │
│Tuesday  │
└Char(9)──┘
~~~

