# standardDeviation

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical{.info}

⍎⍎#.FlipDB.flipdb.TamStatInt.HelpMsg 0 ⍎⍎

~~~
R=[B] standardDeviation X
~~~

* B is a Boolean scalar indicating whether X represents sample or population data. 0=Sample Standard Deviation (default), 1=Population Standard Deviation.
* X is a DataColumn.
* R is the standard deviation of X. R is Float.

See also: →[variance], →[covariance]

## Examples

Sample and population data.

~~~
      standardDeviation 2 3 5 8
┌───────────────┐
│2.6457513110646│
└Float──────────┘
      1 standardDeviation 2 3 5 8
┌───────────────┐
│2.2912878474779│
└Float──────────┘
~~~

The standard deviation is essentially the square root of the variance:

~~~
      standardDeviation 3 5 6 7 9
┌───────────────┐
│2.2360679774998│
└Float──────────┘
      squareRoot variance 3 5 6 7 9
┌───────────────┐
│2.2360679774998│
└Float──────────┘
~~~

When weighting by Integers, you may use the →replicate function

~~~
      standardDeviation 2 8 5 replicate 100 200 300
┌───────────────┐
│67.612340378281│
└Float──────────┘
~~~

Weight the standardDeviation in interest rates by the loan balances:

~~~
      LB=84000 120000 165000
      IR=3.5 4.75 2.3
      standardDeviation LB replicate IR
┌──────────────┐
│1.065417501121│
└Float─────────┘
~~~

