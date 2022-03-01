# lt

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Relational{.info}

Less than.{.purpose}

~~~
R=X lt Y
~~~

X and Y must be numeric. R is boolean. R is 1 if X is less than Y. lt may also be written as <.

## Examples

~~~
      5 lt 7
┌───────┐
│1      │
└Boolean┘
      a=1 2 3 4 5
      b=5 4 3 2 1
      a lt b
┌───────┐
↓1      │
│1      │
│0      │
│0      │
│0      │
└Boolean┘
      b < a
┌───────┐
↓0      │
│0      │
│0      │
│1      │
│1      │
└Boolean┘
~~~

