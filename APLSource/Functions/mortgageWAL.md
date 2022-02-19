# mortgageWAL

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Weighted average life of a mortgage in years.{.purpose}

~~~
R=mortgageWAL I N [C]
~~~

* I is the annualized interest rate expressed as a percentage. Numeric.
* N is the number of months remaining in the mortgage. Numeric.
* C is the constant prepayment rate expressed as a percentage (Optional, default=0). Numeric.

R is the average time to return a principal in years. R is Float.

All inputs should be positive. Unexpected values may occur with negative numbers.

See also: →[averageLife]

## Examples

~~~
      mortgageWAL 9.5 360
┌───────────────┐
│21.337633114142│
└Float──────────┘
~~~

