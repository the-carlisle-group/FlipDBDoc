# addMonths

Applies to:{.prefix}

Date, DateTime{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Add months to a date.{.purpose}

~~~
R=X addMonths Y
~~~

X must be of type Date or DateTime. Y must be type Integer. R is type of X, with Y months added to
X. If Y is negative R is earlier than X. The day element of R is the lower of the number of days
in the month of R and the day element of X or if X is end-of-month R is end-of-month. If X is zero
(null), R is zero.

See also: →[addDays], →[addYears]

## Examples

~~~
      D1='2004-02-29'
      D1 addMonths 11
┌──────────┐
│2005-01-31│
└Date──────┘
      D1 addMonths -2
┌──────────┐
│2003-12-31│
└Date──────┘
~~~

