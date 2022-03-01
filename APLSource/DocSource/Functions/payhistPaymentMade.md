# payhistPaymentMade

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=payhistPaymentMade X
~~~

X is a payment history string. R is a corresponding string of 0's and 1's indicating
if payment was made in that period or not. An invalid payment strings yields an asterisk.

See also: →[payhistValidate], →[payhistClean], →[payhistPaymentsMade], →[payhistTimes30], →[payhistTimes60], →[payhistTimes90]



## Examples

~~~
      payhistPaymentMade '000000000000'
┌────────────┐
│111111111111│
└Char(12)────┘
      payhistPaymentMade '001111230000'
┌────────────┐
│110111001111│
└Char(12)────┘
      payhistPaymentMade '0011X1230000'
┌────────┐
│*       │
└Char(12)┘
~~~

