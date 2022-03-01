# toObservationNumber

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Date/Time,Casting{.info}

~~~
R=[X] toObservationNumber Y
~~~

Y must be a Date or DateTime. X is an optional time interval, and if provided must be one of 'year'
'month' 'day' 'hour' 'minute'  'or 'second'. The result R is Integer.

The observation number R is computed by taking the difference, in the time interval X, of each item
in Y with the earliest date or datetime in Y. This means the earliest DateTime in Y is assigned
observation number 0. Missing DateTimes (zero valued dates) are assigned observation number -1.

Values in Y are only relevant up to the interval specified in X. Finer precision than X is ignored.
For example, when the time interval is 'month' then the dates 2012/12/31 and 2012/12/1 are treated
as the same value.

This primary use of this function is to produce an observation number for the →[lag] and →[lead]
functions, which allows them to work accurately with non-contiguous and unsorted observations.

## Examples:

~~~
      d='2012/04/01,2011/12/31,2012/01/01,2012/02/01'
      d ('month' toObservationNumber d)
 ┌──────────┐  ┌────┐
 ↓2012-04-01│  ↓4   │
 │2011-12-31│  │0   │
 │2012-01-01│  │1   │
 │2012-02-01│  │2   │
 └Date──────┘  └Int8┘
~~~

When the time interval is 'month', as above, the result is equivalent to:

~~~
      d monthsDifference min d
┌────┐
↓4   │
│0   │
│1   │
│2   │
└Int8┘
~~~

Similar equivalent expressions may be constructed for each time interval.

