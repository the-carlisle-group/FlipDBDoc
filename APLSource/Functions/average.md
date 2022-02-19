# average

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical,Date/Time{.info}

Calculates the average value{.purpose}

~~~
R=average X
~~~

X must be numeric or temporal. If X is numeric, R Float. If X is temporal, then R is the same type
as X.

This function is identical to →[mean].

See also: →[weightedAverage]

## Examples

~~~
      i=integers 7
      i (average i)
 ┌────┐  ┌─────┐
 ↓0   │  │3    │
 │1   │  └Float┘
 │2   │
 │3   │
 │4   │
 │5   │
 │6   │
 └Int8┘
      j=integers i
      j (average j)
 ┌─────────────┐  ┌─────┐
 ↓[]           │  ↓0    │
 │[0]          │  │0    │
 │[0,1]        │  │0.5  │
 │[0,1,2]      │  │1    │
 │[0,1,2,3]    │  │1.5  │
 │[0,1,2,3,4]  │  │2    │
 │[0,1,2,3,4,5]│  │2.5  │
 └Int8─────────┘  └Float┘
      b='1950/06/29,1951/04/01,1973/12/04,1979/06/05,1980/11/11'
      b (average b)
 ┌──────────┐  ┌──────────┐
 ↓1950-06-29│  │1967-03-11│
 │1951-04-01│  └Date──────┘
 │1973-12-04│
 │1979-06-05│
 │1980-11-11│
 └Date──────┘
~~~

