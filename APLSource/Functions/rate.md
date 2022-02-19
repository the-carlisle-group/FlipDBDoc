# rate

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

Calculate the periodic interest rate.{.purpose}

~~~
R=[G] rate N P PV [FV[A]]
~~~

* N is the number of periods. Numeric
* P is the fixed payment made each period. Numeric
* PV is the present value. Numeric
* FV is the future value. Numeric (Optional, default=0).
* A is Boolean. 0=Arrears 1=Advanced (Optional, default=0).
* G is initial Guess. Numeric (Optional, default=0.1)

R is the interest rate. The rate function follows the convention that incoming and outgoing
payments are indicated by positive and negative signs respectively. The initial guess is used to
calculate the interest rate iteratively. Interest rates are expressed as decimals, not
percentages. For mortgage use see mortgageRate.

FV and A are optional, but you cannot input A without FV.

See also: →[mortgageRate], →[payment], →[presentValue], →[term]

## Examples

~~~
      rate 20 -780 8000
┌────────────────┐
│0.07420514107176│
└Float───────────┘
      3 round rate 15 -400 4500 500 1
┌──────┐
│0.034 │
└Dec(3)┘
~~~

