# minutesDifference

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Minutes difference between two datetimes.{.purpose}

~~~
R=X minutesDifference Y
~~~

X and Y must be DateTime or Date. R is Integer. R is the number of minutes between the datetimes in
X and Y. A date is treated as a datetime with a zero time element. R will be negative where Y is
greater than X. Where either X or Y is zero (null), R is zero.

See also: →[secondsDifference], →[hoursDifference], →[daysDifference], →[monthsDifference], →[yearsDifference]

## Examples

~~~
      C = currentDateTime
      C (C minutesDifference currentDate)
 ┌───────────────────┐  ┌─────┐
 │2013-02-06 13:02:26│  │782  │
 └DateTime───────────┘  └Int16┘
      C minutesDifference currentDateTime
┌────┐
│(1) │
└Int8┘
~~~

