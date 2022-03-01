# presentValue

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Returns the present value of an investment.{.purpose}

~~~
R=presentValue I N P [FV[A]]
~~~

* I is the interest rate expressed as a decimal. Numeric
* N is the number of periods. Numeric
* P is the payment. Numeric
* FV is the future value. Numeric (Optional, default=0).
* A is Boolean. 0=Arrears 1=Advanced (Optional, default=0).

R is the present value of a series of identical cash flows over a period of time.  For example,
when you borrow money, the loan amount is the present value to the lender. The presentValue
function follows the convention that incoming and outgoing payments are indicated by positive and
negative signs respectively.

FV and A are optional, but you cannot input A without FV.

See also: →[futureValue], →[payment], →[rate], →[term]

## Examples

~~~
      presentValue .05 10 -100
┌───────────────┐
│772.17349291848│
└Float──────────┘
      2 round presentValue .07 15 -200 450 1
┌────────┐
│1,785.99│
└Dec(2)──┘
~~~

