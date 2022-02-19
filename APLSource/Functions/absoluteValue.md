# absoluteValue

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

absolute, unsigned{.purpose}

~~~
R=absoluteValue X
~~~

X must be numeric. R is the absolute value of X.  R is numeric.

~~~
      absoluteValue -3
┌────┐
│3   │
└Int8┘
      absoluteValue 5 0 -3
┌────┐
↓5   │
│0   │
│3   │
└Int8┘
~~~

