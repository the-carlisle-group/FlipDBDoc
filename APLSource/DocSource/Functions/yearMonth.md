# yearMonth

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Date/Time{.info}

~~~
R=yearMonth X
~~~

X must be a Date or DateTime. R is the year and month extracted from the date. R is Integer or the
form ccyymm.

## Examples

~~~
      yearMonth '2009-12-31'
┌──────┐
│200912│
└Int32─┘
~~~

