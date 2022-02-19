# interestPayment

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=interestPayment I A N PV [FV]
~~~

* I is a periodic rate expressed as a decimal.
* A is age (the number of periods in the future). Numeric.
* N is the term of the loan. Numeric.
* PV is the present value. Numeric.
* FV is the future value. Numeric.

R is the interest payable in period A. R is Float. Total payment = Principal payment + interest payment.

See also: →[principalPayment], →[payment]

## Examples

~~~
      interestPayment .07 10 15 20000
┌────────────────┐
│-732.67660629277│
└Float───────────┘
      interestPayment (9.5 divide 1200) 3 360 100000
┌───────────────┐
│-790.8847811811│
└Float──────────┘
~~~

