# yearBegin

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

~~~
R=yearBegin X
~~~

X is Date or DateTime. R has type as of X being the first day of the year for each date in X.

## Examples

~~~
      yearBegin '2009-12-31'
┌──────────┐
│2009-01-01│
└Date──────┘
~~~

