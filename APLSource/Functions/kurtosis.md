# kurtosis

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical{.info}

⍎⍎#.FlipDB.flipdb.TamStatInt.HelpMsg 0 ⍎⍎

~~~
R=kurtosis X
~~~

* X is numeric.
* R is the (excess) kurtosis of X.

Kurtosis is the fourth moment about the mean.  If the data are approximately normal, the excess
kurtosis is close to zero.  Positive skewness for symmetric distributions indicates that the data
are more peaked  about the center and have thicker tails than the normal distribution.  For
example, the Student t Distribution has positive kurtosis but decreases as the  number of degrees
of freedom increases.

## Examples

~~~
      T5=5 tDist randomVariable 200
      kurtosis T5
┌───────────────┐
│1.5060203721697│
└Float──────────┘
      T12=12 tDist randomVariable 200
      kurtosis T12
┌────────────────┐
│0.65180614250134│
└Float───────────┘
      Z=normal randomVariable 200
      kurtosis Z
┌────────────────┐
│0.14127136619117│
└Float───────────┘
~~~

