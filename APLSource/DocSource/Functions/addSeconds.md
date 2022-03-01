# addSeconds

Applies to:{.prefix}

Date, DateTime{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Add seconds to a date.{.purpose}

~~~
R=X addSeconds Y
~~~

X must be Date or DateTime, Y must be Integer. R is DateTime with Y seconds added to X. If Y is
negative R is earlier than X. If X is zero (null), R is zero.

See also: →[addMinutes], →[addHours], →[addDays], →[addMonths], →[addYears]

## Examples

~~~
      C = currentDateTime
      C (C addSeconds 62, -2))
 ┌───────────────────┐  ┌───────────────────┐
 │2013-02-06 14:01:51│  ↓2013-02-06 14:02:53│
 └DateTime───────────┘  │2013-02-06 14:01:49│
                        └DateTime───────────┘
~~~

