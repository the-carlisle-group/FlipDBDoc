# mortgageTerm

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Compute remaining term of a monthly mortgage.{.purpose}

~~~
R=mortgageTerm I P BO [BN]
~~~

* I is the interest rate as a percentage, e.g. 6.25. Numeric.
* P is the monthly payment. Numeric.
* BO is the original balance. Numeric.
* BN is the baloon payment (default 0). Numeric.

R is the remaining term in months. This may be rounded to the next highest integer.

All inputs should be positive. Unexpected values may occur with negative numbers.

See also: →[mortgageBalance], →[mortgagePayment], →[mortgageRate], →[term]

## Examples

~~~
      mortgageTerm 6 537.32 75000
┌─────┐
│240  │
└Int16┘
      Rates=4 6.5 8.3
      Payments=759.34 1491.15 2264.34
      Balances=75000 200000 300000
      Rates Payments Balances
 ┌──────┐  ┌────────┐  ┌───────┐
 ↓4.0   │  ↓759.34  │  ↓75,000 │
 │6.5   │  │1,491.15│  │200,000│
 │8.3   │  │2,264.34│  │300,000│
 └Dec(1)┘  └Dec(2)──┘  └Int32──┘
      mortgageTerm Rates Payments Balances
┌─────┐
↓120  │
│240  │
│360  │
└Int16┘
~~~

