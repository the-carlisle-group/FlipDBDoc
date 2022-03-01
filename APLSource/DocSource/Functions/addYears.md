# addYears

Applies to:{.prefix}

Date, DateTime{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Add years to a Date or DateTime.{.purpose}

~~~
R=X addYears Y
~~~

X must be of type Date or DateTime. Y must be type Integer. R is type of X with Y years added to X.
If Y is negative R is earlier than X. The day element of R is decremented where X is a leap day
and R is NOT a leap year.  If X is zero (null), R is zero.

See also: →[addDays], →[addYears]

## Examples

~~~
      D1='2004-02-29'
      D1 addYears 2
┌──────────┐
│2006-02-28│
└Date──────┘
      D1 addYears -2
┌──────────┐
│2002-02-28│
└Date──────┘
~~~

