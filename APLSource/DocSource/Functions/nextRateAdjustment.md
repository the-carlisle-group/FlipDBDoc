# nextRateAdjustment

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=[B] nextRateAdjustment D1 D2 F
~~~

* D1 is the Reference Rate Adjustment Date.
* D2 is the As Of Date.
* F is Integer frequency in months.
* B is a Boolean (optional). 0=Actual Rate Adjustment date (default), 1=First of following month.

R is the next rate adjustment date. R is Date.

See also: →[mortgageRollBalance]

## Examples

~~~
      nextRateAdjustment '1997/01/01' '1997/04/01' 6
┌──────────┐
│1997-07-01│
└Date──────┘
~~~

