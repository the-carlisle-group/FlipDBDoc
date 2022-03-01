# futureValue

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=futureValue I N P [PV[A]]
~~~

* I is the periodic interest rate expressed in decimal form.
* N is the total number of payment periods. Numeric.
* P is the fixed payment made each period. Numeric.
* PV is the present value. Numeric (Optional).
* A is Boolean. 0=Arrears 1=Advanced (Optional).

R is the future value (remaining balance). R is Float. The futureValue function follows the
convention that incoming and outgoing payments are indicated by positive and negative signs respectively.

PV and A are optional, but you cannot input A without PV.

See also: →[mortgageBalance], →[presentValue]

## Examples

~~~
      futureValue 0.007 48 -350
┌────────────────┐
│19885.0999943027│
└Float───────────┘
      futureValue (.05 divide 12) 180 -400 1000 1
┌────────────────┐
│105247.355154637│
└Float───────────┘
~~~

