# rankDown

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Statistical{.info}

~~~
R=[Y] rankDown X
~~~
X is any column type except Blob.
R is the corresponding rank of each item in X, ranked in descending order.
Y is 0 (the default) 1 or 2, and provides 3 ranking algorithms: rank, dense rank, and average rank.

See also: →[rankUp] →[number] →[percentileRank]

Use the →[group] function and the →[*.by] operator to rank within groups.

## Examples
~~~
      A= 85 80 85 72 71 99 72 85 98 89
      A (rankDown A) (1 rankDown A) (2 rankDown A)
 ┌────┐  ┌────┐  ┌────┐  ┌─────┐
 ↓85  │  ↓3   │  ↓3   │  ↓4    │
 │80  │  │6   │  │4   │  │6    │
 │85  │  │3   │  │3   │  │4    │
 │72  │  │7   │  │5   │  │7.5  │
 │71  │  │9   │  │6   │  │9    │
 │99  │  │0   │  │0   │  │0    │
 │72  │  │7   │  │5   │  │7.5  │
 │85  │  │3   │  │3   │  │4    │
 │98  │  │1   │  │1   │  │1    │
 │89  │  │2   │  │2   │  │2    │
 └Int8┘  └Int8┘  └Int8┘  └Float┘

       A (rankUp A) (1 rankUp A) (2 rankUp A)
 ┌────┐  ┌────┐  ┌────┐  ┌─────┐
 ↓85  │  ↓4   │  ↓3   │  ↓5    │
 │80  │  │3   │  │2   │  │3    │
 │85  │  │4   │  │3   │  │5    │
 │72  │  │1   │  │1   │  │1.5  │
 │71  │  │0   │  │0   │  │0    │
 │99  │  │9   │  │6   │  │9    │
 │72  │  │1   │  │1   │  │1.5  │
 │85  │  │4   │  │3   │  │5    │
 │98  │  │8   │  │5   │  │8    │
 │89  │  │7   │  │4   │  │7    │
 └Int8┘  └Int8┘  └Int8┘  └Float┘

~~~

