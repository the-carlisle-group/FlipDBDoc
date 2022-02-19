# payhistValidate

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=payhistValidate X
~~~

X is character. R is boolean indicating if the string is a valid payment history string or not.
A valid payment history string consists entirely of the digits 0-9, where reading from left to right,
digits never increase by more than 1. In other words, digits may decrease by any amount,
stay the same, or increase by one.

See also:  →[payhistClean], →[payhistPaymentsMade], →[payhistPaymentMade], →[payhistTimes30], →[payhistTimes60], →[payhistTimes90]

## Examples

~~~
      payhistValidate '000012500000'
┌───────┐
│0      │
└Boolean┘
      payhistValidate '000012300000'
┌───────┐
│1      │
└Boolean┘
      payhistValidate '0X0012300000'
┌───────┐
│0      │
└Boolean┘
~~~

