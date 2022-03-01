# yearsDifference

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Years difference between two dates.{.purpose}

~~~
R=X yearsDifference Y
~~~

X and Y must be Date or DateTime. R is Integer. R is the number of years between the starts of the
years in X and Y. R will be negative if Y is greater than X. If either X or Y is zero (null), R is zero.

See also: →[daysDifference], →[monthsDifference]

## Examples

~~~
      '2012-10-11' yearsDifference '1964-7-24'
┌────┐
│48  │
└Int8┘
~~~

