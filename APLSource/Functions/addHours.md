# addHours

Applies to:{.prefix}

Date, DateTime{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Add hours to a date.{.purpose}

~~~
R=X addHours Y
~~~

X must be Date or DateTime, Y must be Integer. R is DateTime with Y hours added to X. If Y is
negative R is earlier than X. If X is zero (null), R is zero.

See also: →[addSeconds], →[addMinutes], →[addDays], →[addMonths], →[addYears]

## Examples

~~~
      C = currentDateTime
      C (C addHours 12 -2)
 ┌───────────────────┐  ┌───────────────────┐
 │2013-02-06 13:49:39│  ↓2013-02-07 01:49:39│
 └DateTime───────────┘  │2013-02-06 11:49:39│
                        └DateTime───────────┘
~~~

