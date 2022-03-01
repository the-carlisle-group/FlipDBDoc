# addMinutes

Applies to:{.prefix}

Date, DateTime{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Add minutes to a date.{.purpose}

~~~
R=X addMinutes Y
~~~

X must be Date or DateTime, Y must be Integer. R is DateTime with Y minutes added to X. If Y is
negative R is earlier than X. If X is zero (null), R is zero.

See also: →[addSeconds], →[addHours], →[addDays], →[addMonths], →[addYears]

## Examples

~~~
      C = currentDateTime
      C (C addMinutes 62 -2)
 ┌───────────────────┐  ┌───────────────────┐
 │2013-02-06 13:49:39│  ↓2013-02-06 14:51:39│
 └DateTime───────────┘  │2013-02-06 13:47:39│
                        └DateTime───────────┘
~~~

