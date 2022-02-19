# toTime

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=toTime X
~~~

X must be DateTime or Integer. If X is DateTime, R is the time portion without the date. Unknown or
missing datetimes in X convert to time zero "00:00:00".

If X is integer each number represents a time as "hhmmss". Any integers in X that do not represent
valid times such as 106100 (more than 59 minutes) or 251216 (more than 24 hours) will convert to
time zero "00:00:00".

## Examples

~~~
      toTime 165959
┌────────┐
│16:59:59│
└Time────┘
      toTime '1999-12-31 22:59:59'
┌────────┐
│22:59:59│
└Time────┘
      toTime 99
┌────────┐
│00:00:00│
└Time────┘
~~~

