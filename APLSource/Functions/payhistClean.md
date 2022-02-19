# payhistClean

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=payhistClean X
~~~

X is mortgage payment history string. R is X forced to be valid. A worst-case scenario is assumed.
Non-digit characters, including non-trailing blanks, are assumed to be 9. If two consective digits conflict, the larger is assumed to be correct,
and the digits leading up the larger digit are increased accordingly.
It is often useful to replace specific characters with specfice digits in the string before applying this function.

See also: →[payhistValidate], →[payhistPaymentsMade], →[payhistPaymentMade], →[payhistTimes30], →[payhistTimes60], →[payhistTimes90]

## Examples

~~~

      payhistClean'000012500000'
┌────────────┐
│001234500000│
└Char(12)────┘

      payhistClean'00 0125000000000X'
┌─────────────────┐
│78923450123456789│
└Char(17)─────────┘

~~~

