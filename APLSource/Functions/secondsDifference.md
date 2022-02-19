# secondsDifference

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Seconds difference between two datetimes.{.purpose}

~~~
R=X secondsDifference Y
~~~

X and Y must be DateTime or Date. R is Integer. R is the number of seconds between the datetimes in
X and Y. A date is treated as a datetime with a zero time element. R will be negative where Y is
greater than X. Where either X or Y is zero (null), R is zero.

See also: →[minutesDifference], →[hoursDifference], →[daysDifference], →[monthsDifference], →[yearsDifference]

## Examples

~~~
      C = currentDateTime
      C (C secondsDifference currentDate)
 ┌───────────────────┐  ┌─────┐
 │2010-12-02 18:06:08│  │65168│
 └DateTime───────────┘  └Int32┘
      sum 18 6 8 times 3600 60 1
┌─────┐
│65168│
└Int32┘
      currentDateTime secondsDifference C
┌────┐
│21  │
└Int8┘
~~~

