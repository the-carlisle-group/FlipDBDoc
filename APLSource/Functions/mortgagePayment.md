# mortgagePayment

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Determine the monthly payment based on the terms of a mortgage.{.purpose}

~~~
R=mortgagePayment I N BO
~~~

* I is the interest rate expressed as a percentage. Numeric.
* N is the number of months. Numeric.
* BO is the original balance of the mortgage. Numeric.

R is the mortgage payment. R is rounded to 2 decimal places.

All inputs should be positive. Unexpected values may occur with negative numbers.

See also: →[mortgageBalance], →[mortgageRate], →[mortgageTerm], →[payment]

## Examples

~~~
      mortgagePayment 6 240 75000
┌──────┐
│537.32│
└Dec(2)┘
      Rates=4 6.5 8.3
      Terms=120 240 360
      Balances=75000 200000 300000
      Rates Terms Balances
 ┌──────┐  ┌─────┐  ┌───────┐
 ↓4.0   │  ↓120  │  ↓75,000 │
 │6.5   │  │240  │  │200,000│
 │8.3   │  │360  │  │300,000│
 └Dec(1)┘  └Int16┘  └Int32──┘
      mortgagePayment Rates Terms Balances
┌────────┐
↓759.34  │
│1,491.15│
│2,264.35│
└Dec(2)──┘
~~~

