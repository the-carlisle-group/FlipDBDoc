# mortgageCPR

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Determine the constant prepayment rate by comparing the actual balance to the expected balance.{.purpose}

~~~
R=mortgageCPR BO BN I A N
~~~

* BO is the original balance. Numeric
* BN is the current balance (0=paid off). Numeric.
* I is the interest rate. Numeric.
* A is age of the mortgage in months. Numeric.
* N is the number of monthly periods. Numeric.

R is the constant prepayment rate. This is a Float (0=calculate).

All inputs should be positive. Unexpected values may occur with negative numbers.

See also: →[presentValue], →[rate], →[term]

## Examples

~~~
        mortgageCPR 100000 93420 9.5 12 360
┌───────────────┐
│6.0003592821417│
└Float──────────┘
~~~

