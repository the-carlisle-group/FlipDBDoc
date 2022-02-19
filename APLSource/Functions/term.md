# term

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Compute number of periods in the loan.{.purpose}

~~~
R=term I P B [FV[A]]
~~~

* I is the interest rate as a decimal.
* P is the periodic payment. Numeric
* B is the balance. Numeric
* FV is the future value (Optional, default $1). Numeric
* A is Boolean. 0=Arrears 1=Advanced (Optional, default=0).

R is the number of payment periods. The term function follows the convention that incoming and
outgoing payments are indicated by positive and negative signs respectively. For mortgage use see mortgageTerm.

FV and A are optional, but you cannot input A without FV.

See also: →[mortgageTerm], →[payment], →[presentValue], →[rate]

## Examples

~~~
     term .0742 -780 8000
┌────────────────┐
│19.9982542750952│
└Float───────────┘
~~~

