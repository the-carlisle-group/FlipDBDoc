# delinquency

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Financial{.info}

~~~
R=[X] delinquency Y Z
~~~

Y is the as-of date, Z is the next payment due date. R is Integer and is the number of months
(number of times 30 days) delinquent. X is 1 (the default) for MBA (Mortgage Bankers Association)
standards, or 0 for OTS (Office of Thrift Supervision).

## Examples

MBA considers a loan delinquent if the payment has not been made by the day before the following
due date:

~~~
      delinquency '2014/7/31' '2014/6/1'
┌────┐
│2   │
└Int8┘
~~~

OTS considers a loan delinquent if the payment has not made by the following due date, thus giving
one more day to make the payment than MBA:

~~~
      0 delinquency '2014/7/31' '2014/6/1'
┌────┐
│1   │
└Int8┘
~~~

As-Of dates may be any day of the month - not necessarily the end of month, and next due dates may
also be any day of the month, not necessarily the 1st. See the user guide for a full discussion of
delinquency computations and the exact algorithm used to implement this function.

