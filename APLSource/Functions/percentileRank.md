# percentileRank

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Statistical{.info}

~~~
R=[X] percentileRank Y [Z]
~~~

* Y is numeric.
* X is an optional Boolean flag.
* R is the percentile rank of the corresponding item in Y. R is Float.
* Z is an optional population column.

If X is 0, the default, this function emulates Excel's PERCENTRANK.INC.
If X is 1, this function emulates Excel's PERCENTRANK.EXC.

If Z is not provided, then Y defines both the values for which
we want to compute percentile rank, and the entire population for
determining the rank. If Z is provided, then it defines the population
for determining the rank. In this case, values in Y may not be
found in Z, and linear interpolation is used. Y and Z are generally not corresponding columns of the same length.

Note that while Excel returns a number between 0 and 1, FlipDB
returns numbers between 0 and 100.

## Examples

~~~
      A=1 2 3 4 5 6 7 8 9 10
      A (percentileRank A) (1 percentileRank A)
 ┌────┐  ┌────────────┐  ┌────────────┐
 ↓1   │  ↓  0         │  ↓ 9.090909091│
 │2   │  │ 11.11111111│  │18.18181818 │
 │3   │  │ 22.22222222│  │27.27272727 │
 │4   │  │ 33.33333333│  │36.36363636 │
 │5   │  │ 44.44444444│  │45.45454545 │
 │6   │  │ 55.55555556│  │54.54545455 │
 │7   │  │ 66.66666667│  │63.63636364 │
 │8   │  │ 77.77777778│  │72.72727273 │
 │9   │  │ 88.88888889│  │81.81818182 │
 │10  │  │100         │  │90.90909091 │
 └Int8┘  └Float───────┘  └Float───────┘

      percentileRank 99 75 87
┌─────┐
↓100  │
│  0  │
│ 50  │
└Float┘

      percentileRank (99 75 87) (99 75 87)
┌─────┐
↓100  │
│  0  │
│ 50  │
└Float┘

      percentileRank (99 75 87) (1+integers 100)
┌───────────┐
↓98.98989899│
│74.74747475│
│86.86868687│
└Float──────┘

 ~~~

