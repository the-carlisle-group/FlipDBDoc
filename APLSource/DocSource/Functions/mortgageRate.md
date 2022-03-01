# mortgageRate

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Find the annual mortgage rate (nominal rate) in percentage terms.{.purpose}

~~~
R=mortgageRate N P B
~~~

* N is the  number of monthly periods. Numeric.
* P is the monthly payment. Numeric.
* B is the balance. Numeric.

R is the coupon rate as a percentage. R is rounded to three decimal places.

All inputs should be positive. Unexpected values may occur with negative numbers.

See also: →[mortgageBalance], →[mortgagePayment], →[mortgageTerm], →[rate]

## Examples

~~~
      mortgageRate 360 840.85 100000
┌──────┐
│9.500 │
└Dec(3)┘
      Terms=120 240 360
      Payments=759.34 1491.15 2264.34
      Balances=75000 200000 300000
      Terms Payments Balances
 ┌─────┐  ┌────────┐  ┌───────┐
 ↓120  │  ↓759.34  │  ↓75,000 │
 │240  │  │1,491.15│  │200,000│
 │360  │  │2,264.34│  │300,000│
 └Int16┘  └Dec(2)──┘  └Int32──┘
      mortgageRate Terms Payments Balances
┌──────┐
↓4.000 │
│6.500 │
│8.300 │
└Dec(3)┘
~~~

