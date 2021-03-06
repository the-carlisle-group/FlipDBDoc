# variance

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical{.info}

⍎⍎#.FlipDB.flipdb.TamStatInt.HelpMsg 0 ⍎⍎

~~~
R=[B] variance X
~~~
* B is 0 for sample variance, 1 for population variance. B defaults to 0.
* X is a DataColumn.
* R is a scalar unless the input is a partitioned DataColumn in which case R is a simple DataColumn. R is Float.

See also: →[covariance], →[standardDeviation]

## Examples

Sample Variance:

~~~
      variance 2 3 5 8
┌─────┐
│7    │
└Float┘
~~~

Population Variance:

~~~
      1 variance 2 3 5 8
┌─────┐
│5.25 │
└Float┘
~~~

Add a weighting factor.  Weight the variance in interest rates by the loan balances:

~~~
      LB=84000 120000 165000
      IR=3.5 4.75 2.3
      variance LB replicate IR
┌───────────────┐
│1.1351144516949│
└Float──────────┘
~~~

Suppose we have 15 items.  2 are priced at $100, 8 at $200 and 5 at $300.  Find the variance:

~~~
      Price=100 200 300
      Quan=2 8 5
      variance Quan replicate Price
┌───────────────┐
│4571.4285714286│
└Float──────────┘
~~~

