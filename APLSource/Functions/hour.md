# hour

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Extracts hour from Time or DateTime.{.purpose}

~~~
R=hour X
~~~

X must be Time or DateTime. R is the hour extracted from the time. R is Integer.

## Examples

~~~
    hour '2009-02-21 13:23:11,2009-02-21 09:11:00'
┌────┐
↓13  │
│ 9  │
└Int8┘
~~~

