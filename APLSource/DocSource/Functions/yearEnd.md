# yearEnd

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

~~~
R=yearEnd X
~~~

X is Date or DateTime. R has type as of X being the last day of the year for each date in X.

## Examples

~~~
      yearEnd '2009-12-31'
┌──────────┐
│2009-12-31│
└Date──────┘
~~~

