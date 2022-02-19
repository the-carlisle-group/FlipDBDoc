# month

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

Extracts month from Date or DateTime{.purpose}

~~~
R=month X
~~~

X  must be a Date or DateTime. R is the month extracted from the date. R is Integer.

## Examples

~~~
      month '2009-02-14,2000-01-01'
┌────┐
↓2   │
│1   │
└Int8┘
      month '2009-02-14 00:01:02,2000-01-01 23:59:59'
┌────┐
↓2   │
│1   │
└Int8┘
      month emptyDate
┌────┐
↓Int8┘
~~~

