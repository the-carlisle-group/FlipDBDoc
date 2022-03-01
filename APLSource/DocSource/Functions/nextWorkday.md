# nextWorkday

Applies to:{.prefix}

Date{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Find the next working day{.purpose}

~~~
R=[X] nextWorkday Y
~~~

Y is Date. R is also Date having identical structure to Y and represents the next working day after
each date in Y. Dates in R exclude weekends or holidays.

X is an optional scalar or simple Char and represents one or more country codes; the default is
'US'. Dates in R exclude weekends or holidays in any country included in X.

See also: →[*.addDays],→[*.previousWorkday]

## Examples

~~~
      D='2010-12-23'addDays integers 10
      N=nextWorkday D
      D(weekday D)N(weekday N)
 ┌──────────┐  ┌─────────┐  ┌──────────┐  ┌─────────┐
 ↓2010-12-23│  ↓Thursday │  ↓2010-12-27│  ↓Monday   │
 │2010-12-24│  │Friday   │  │2010-12-27│  │Monday   │
 │2010-12-25│  │Saturday │  │2010-12-27│  │Monday   │
 │2010-12-26│  │Sunday   │  │2010-12-27│  │Monday   │
 │2010-12-27│  │Monday   │  │2010-12-28│  │Tuesday  │
 │2010-12-28│  │Tuesday  │  │2010-12-29│  │Wednesday│
 │2010-12-29│  │Wednesday│  │2010-12-30│  │Thursday │
 │2010-12-30│  │Thursday │  │2011-01-03│  │Monday   │
 │2010-12-31│  │Friday   │  │2011-01-03│  │Monday   │
 │2011-01-01│  │Saturday │  │2011-01-03│  │Monday   │
 └Date──────┘  └Char(9)──┘  └Date──────┘  └Char(9)──┘
~~~

