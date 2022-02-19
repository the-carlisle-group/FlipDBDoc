# interQuartileRange

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Statistical{.info}

Difference between the 3rd and 1st Quartiles of a data set.{.purpose}

~~~
R=interQuartileRange X [W]
~~~

X must be numeric.  R is the variance of X.

W is a weighting vector.  Must be Integer.

See also: →[range], →[percentile]

## Examples

~~~
      interQuartileRange 8 12 7 17 14 45 10 13 17 13 9 11
┌─────┐
│6    │
└Float┘
~~~

