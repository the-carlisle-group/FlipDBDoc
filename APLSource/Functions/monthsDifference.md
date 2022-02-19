# monthsDifference

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Months difference between two dates.{.purpose}

~~~
R=X monthsDifference Y
~~~

X and Y must be Date or DateTime. R is Integer. R is the number of months between the starts of the
months in X and Y. Note that no account is taken of the number of days in the date, and so the
number of months between the last day of, say, January, and the first day of February is 1. R will
be negative if Y is greater than X. If either X or Y is zero (null), R is zero.

See also: →[daysDifference], →[yearsDifference]

## Examples

~~~
    '2001-12-1' monthsDifference  '2001-11-30'
┌────┐
│1   │
└Int8┘
     '2001-12-1' monthsDifference '2002-12-1'
┌────┐
│(12)│
└Int8┘
~~~

