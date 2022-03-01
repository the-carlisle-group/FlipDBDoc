# previousWorkday

Applies to:{.prefix}

Date{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Find the previous working day{.purpose}

~~~
R=[X] previousWorkday Y
~~~

Y is Date. R is also Date having identical structure to Y and represents the previous working day
before each date in Y. Dates in R exclude weekends or holidays.

X is an optional scalar or simple Char and represents one or more country codes; the default is
'US'. Dates in R exclude weekends or holidays in any country included in X.

See also: →[addDays], →[nextWorkday]

## Examples

~~~
      D='2010-12-25'addDays integers 10
      P=previousWorkday D
      D(weekday D)P(weekday P)
 ┌──────────┐  ┌─────────┐  ┌──────────┐  ┌─────────┐
 ↓2010-12-25│  ↓Saturday │  ↓2010-12-23│  ↓Thursday │
 │2010-12-26│  │Sunday   │  │2010-12-23│  │Thursday │
 │2010-12-27│  │Monday   │  │2010-12-23│  │Thursday │
 │2010-12-28│  │Tuesday  │  │2010-12-27│  │Monday   │
 │2010-12-29│  │Wednesday│  │2010-12-28│  │Tuesday  │
 │2010-12-30│  │Thursday │  │2010-12-29│  │Wednesday│
 │2010-12-31│  │Friday   │  │2010-12-30│  │Thursday │
 │2011-01-01│  │Saturday │  │2010-12-30│  │Thursday │
 │2011-01-02│  │Sunday   │  │2010-12-30│  │Thursday │
 │2011-01-03│  │Monday   │  │2010-12-30│  │Thursday │
 └Date──────┘  └Char(9)──┘  └Date──────┘  └Char(9)──┘
~~~

