# le

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Relational{.info}

Less than or equal to.{.purpose}

~~~
R=X le Y
~~~

X and Y must be numeric. R is boolean and is 1 if X is less than or equal to Y. This function may
also be written as <=.

## Examples

~~~
      a=1 2 3 4 5
      b=5 4 3 2 1
      a le b
┌───────┐
↓1      │
│1      │
│1      │
│0      │
│0      │
└Boolean┘
      b <= a
┌───────┐
↓0      │
│0      │
│1      │
│1      │
│1      │
└Boolean┘
~~~

