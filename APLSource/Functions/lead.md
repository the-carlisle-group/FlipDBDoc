# lead

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Misc{.info}

~~~
R=lead X Y Z [A [B]]
~~~

X must be a column expression. Y must be a scalar Integer. Z must be a scalar value appropriate as
a fill element for the type of X.

When only X,Y and Z are provided, R is X shifted up Y rows, with the Y vacant rows filled with Z.

A and B are optional. If A but not B is provided, A must be Integer representing observation
numbers corresponding to values in X. The result R is then X where the observation number is equal
to A-Y. If no observation number is found, the result is filled with Z. Observation numbers may
not be found due to missing intermediate observations in addition to the usual case where the last
Y values by definition are not available. Observation numbers may be computed using the
→[toObservationNumber] function.

If A and B are both provided, A must be a Date or DateTime, and B is Char and must be one of 'year'
'month' 'day' 'hour' 'minute' or 'second'. A and B are used to compute an observation number and
the result R is just as immediately above.

## Examples

~~~
      B1=1 2 3 4 5
      lead B1 2 -1
┌────┐
↓ 3  │
│ 4  │
│ 5  │
│-1  │
│-1  │
└Int8┘
      B1='Mike,Tim,Jon,Henry'
      lead B1 2 '***'
┌───────┐
↓Jon    │
│Henry  │
│***    │
│***    │
└Char(5)┘
~~~

