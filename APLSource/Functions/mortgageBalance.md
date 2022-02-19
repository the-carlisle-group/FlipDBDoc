# mortgageBalance

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Find the balance of a mortgage at any point in time.{.purpose}

~~~
R=[C] mortgageBalance BO I A [N]
~~~

* BO is the original balance of the mortgage. Numeric.
* I is the interest rate expressed as a percentage. Numeric.
* A is the number of months in the future. Numeric.
* N is the number of payments. Numeric.(Optional, default = 360)
* C is the constant prepayment rate. Numeric. (Optional default = 0)

R is the remaining balance (futureValue). This function uses the mortgage term instead of the
monthly payment. If the monthly payment is available, use the futureValue function. Result is Dec(2).

All inputs should be positive. Unexpected values may occur with negative numbers.

See also: →[futureValue], →[mortgagePayment], →[mortgageRate], →[mortgageTerm], →[mortgageRollBalance]

## Examples

~~~
      6 mortgageBalance 100000 9.5 5 360
┌─────────┐
│97,211.31│
└Dec(2)───┘
      mortgageBalance (60000 30000 100000) (5 7 9.5) (50 100 300) (240 180 360)
┌─────────┐
↓51,904.06│
│17,198.65│
│40,037.13│
└Dec(2)───┘
~~~

