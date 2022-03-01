# payhistTimes90

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=Y payhistTimes90 X
~~~

X is a payment history string. Y is an optional number of digits from the end of the string to analyze.
Y defaults to the length of the entire string. R is the number of times 90 days or more delinquent in the last
Y months. R is integer.

See also: →[payhistValidate], →[payhistClean], →[payhistPaymentsMade], →[payhistPaymentMade], →[payhistTimes30], →[payhistTimes60]

## Examples

~~~
      payhistTimes90 '001111233300000000'
┌────┐
│3   │
└Int8┘
      6 payhistTimes90 '001111233300000000'
┌────┐
│0   │
└Int8┘
~~~

