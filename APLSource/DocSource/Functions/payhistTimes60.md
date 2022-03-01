# payhistTimes60

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=Y payhistTimes60 X
~~~

X is a payment history string. Y is an optional number of digits from the end of the string to analyze.
Y defaults to the length of the entire string. R is the number of times 60 or more days delinquent in the last
Y months. R is integer.

See also: →[payhistValidate], →[payhistClean], →[payhistPaymentsMade], →[payhistPaymentMade], →[payhistTimes30], →[payhistTimes90]

## Examples

~~~
       payhistTimes60 '00111123000000000'
┌────┐
│2   │
└Int8┘
       6 payhistTimes60 '00111123000000000'
┌────┐
│0   │
└Int8┘

~~~

