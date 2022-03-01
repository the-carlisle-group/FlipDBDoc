# minute

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Extracts minute from Time or DateTime.{.purpose}

~~~
R=minute X
~~~

X must be Time or DateTime. R is the minute extracted from the time. R is Integer.

## Examples

~~~
      minute '2004-02-29 12:34:56,2004-02-29 00:00:56'
┌────┐
↓34  │
│ 0  │
└Int8┘
      minute '12:34:56,00:00:56'
┌────┐
↓34  │
│ 0  │
└Int8┘
~~~

