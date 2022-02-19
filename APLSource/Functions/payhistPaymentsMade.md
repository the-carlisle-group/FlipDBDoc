# payhistPaymentsMade

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=payhistPaymentsMade X
~~~

X is a payment history string. R is a corresponding string indicating the number of
payments made in each period.  An invalid payment strings yields an asterisk.

See also: →[payhistValidate], →[payhistClean], →[payhistPaymentMade], →[payhistTimes30], →[payhistTimes60], →[payhistTimes90]


## Examples

~~~
       payhistPaymentsMade '00000000000'
┌───────────┐
│11111111111│
└Char(11)───┘
       payhistPaymentsMade '001111230000'
┌────────────┐
│110111004111│
└Char(12)────┘
       payhistPaymentsMade '0011X1230000'
┌────────┐
│*       │
└Char(12)┘
~~~

