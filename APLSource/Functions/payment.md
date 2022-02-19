# payment

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=payment I N PV [FV[A]]
~~~

* I is the periodic interest rate expressed in decimal form.
* N is number of periods. Numeric
* PV is the present value (Optional, default $1). Numeric
* FV is the future value (Optional, default 0). Numeric
* A is Boolean. 0 is advanced, 1 is arrears.

R is the payment made each period. The payment function follows the convention that incoming and
outgoing payments are indicated by positive and negative signs respectively. For mortgage use see mortgagePayment.

FV and A are optional, but you cannot input A without FV.

See also: →[mortgagePayment], →[presentValue], →[rate], →[term]

## Examples

~~~
      payment (8 divide 1200) 10 10000
┌─────────────────┐
│-1037.03208935916│
└Float────────────┘
      payment .004167 60 25000 3000 1
┌────────────────┐
│-513.75784432311│
└Float───────────┘
~~~

