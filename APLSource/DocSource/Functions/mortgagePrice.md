# mortgagePrice

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Determine the price of a $100 mortgage based on the yield.{.purpose}

~~~
R=mortgagePrice Y C I N
~~~

* Y is the return on investment (yield). Numeric.
* C is the constant prepayment rate. Numeric.
* I is the interest rate. Numeric.
* N is the number of monthly periods. Numeric.

R is the price per $100 par value. R is rounded to two decimal places.

All inputs should be positive. Unexpected values may occur with negative numbers.

See also: →[mortgageYield]

## Examples

~~~
      mortgagePrice 8.25 6 9.5 360
┌────────────────┐
│107.875720386707│
└Float───────────┘
~~~

