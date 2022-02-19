# mean

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical,Date/Time{.info}

Calculates the mean or average value.{.purpose}

⍎⍎#.FlipDB.flipdb.TamStatInt.HelpMsg 0 ⍎⍎ For temporal input, however, this function uses flipDB's →[average] function

~~~
R=mean X
~~~

X is a numeric or temporal DataColumn.
R is a scalar Float.
If X is numeric, R is Float. If X is temporal, then R is also temporal.

See also: →[weightedAverage], →[average]

~~~
      RATE=4.25 3.75 4.25 3.875 4.325
      mean RATE
┌─────┐
│4.09 │
└Float┘
~~~

Partitioned data:

~~~
      mean enclose (2 3 5 7)(1 8 2)
┌───────────────┐
↓4.25           │
│3.6666666666667│
└Float──────────┘
      i=integers 7
      j=integers i
      j (mean j)
 ┌─────────────┐  ┌─────┐
 ↓[]           │  ↓0    │
 │[0]          │  │0    │
 │[0,1]        │  │0.5  │
 │[0,1,2]      │  │1    │
 │[0,1,2,3]    │  │1.5  │
 │[0,1,2,3,4]  │  │2    │
 │[0,1,2,3,4,5]│  │2.5  │
 └Int8─────────┘  └Float┘
~~~

Temporal data:

~~~
      b='1950/06/29,1951/04/01,1973/12/04,1979/06/05,1980/11/11'
      b (mean b)
 ┌──────────┐  ┌──────────┐
 ↓1950-06-29│  │1967-03-11│
 │1951-04-01│  └Date──────┘
 │1973-12-04│
 │1979-06-05│
 │1980-11-11│
 └Date──────┘
~~~

