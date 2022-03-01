# second

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Extracts second from Time or DateTime.{.purpose}

~~~
R=second X
~~~

X must be Time or DateTime. R is the second extracted from the time. R is Integer.

## Examples

~~~
      second '09:12:34'
┌────┐
│34  │
└Int8┘
~~~

