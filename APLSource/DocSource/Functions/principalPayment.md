# principalPayment

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=principalPayment I A N PV [FV]
~~~

* I is coupon rate expressed as a fraction. Numeric.
* A is age (the number of periods in the future). Numeric.
* N is the term of the loan. Numeric.
* PV is the present value. Numeric.
* FV is the future value. Numeric.

R is the principal repayment in period A. R is Float. Total payment = Principal payment + interest payment.

See also: →[interestPayment], →[payment]

## Examples

~~~
      principalPayment .07 10 15 20000
┌─────────────────┐
│-1463.21588772736│
└Float────────────┘
      principalPayment (9.5 divide 1200) 3 360 100000
┌─────────────────┐
│-49.9694259976435│
└Float────────────┘
~~~

