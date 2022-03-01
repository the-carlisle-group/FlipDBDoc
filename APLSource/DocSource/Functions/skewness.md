# skewness

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical{.info}

⍎⍎#.FlipDB.flipdb.TamStatInt.HelpMsg 0 ⍎⍎

~~~
R=skewness X
~~~

X is numeric.  R is the skewness of X.

Skewness is the third moment and measures asymmetry of a data set.  If the data are roughly
symmetrical, the skewness is close to zero.  Right-skewed data have positive skewness, and
left-skewed data have negative skewness.

## Examples

~~~
      A=1 2 3 3 4 5
      skewness A
┌───────┐
│0      │
└Boolean┘
      B=1 2 4 5 5 6 6 6
      skewness B
┌────────────────┐
│-1.0327146006952│
└Float───────────┘
      C=0 0 0 1 1 2
      skewness C
┌────────────────┐
│0.85732140997411│
└Float───────────┘
~~~

