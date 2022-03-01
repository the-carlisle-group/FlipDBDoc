# mortgageYield

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Return on investment to lender in percent.{.purpose}

~~~
R=mortgageYield P C I N
~~~

* P is the price per $100 par value. Numeric.
* C is the constant prepayment rate. Numeric.
* I is the interest rate. Numeric.
* N is the number of monthly periods. Numeric.

R is the return on investment (yield). R is Float.

All inputs should be positive. Unexpected values may occur with negative numbers.

See also: →[mortgagePrice]

## Examples

~~~
      mortgageYield 107.88 6 9.5 360
┌──────────────┐
│8.249366620897│
└Float─────────┘
~~~

