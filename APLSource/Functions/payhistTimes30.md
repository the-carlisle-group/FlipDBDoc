# payhistTimes30

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=Y payhistTimes30 X
~~~

X is a payment history string. Y is an optional number of digits from the end of the string to analyze.
Y defaults to the length of the entire string. R is the number of times 30 days or more delinquent in the last
Y months. R is integer.

See also: →[payhistValidate], →[payhistClean], →[payhistPaymentsMade], →[payhistPaymentMade], →[payhistTimes60], →[payhistTimes90]

## Examples

~~~
      payhistTimes30 '00001220000000000'
┌────┐
│3   │
└Int8┘
      12 payhistTimes30 '0000111000000000000'
┌────┐
│0   │
└Int8┘

~~~

