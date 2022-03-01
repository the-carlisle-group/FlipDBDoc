# floor

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=floor X
~~~

X must be numeric. R is the highest integer not greater than X.  R is Integer.

## Examples

~~~
      floor 3
┌────┐
│3   │
└Int8┘
      floor 3.9 -3.9
┌────┐
↓3   │
│(4) │
└Int8┘
~~~

