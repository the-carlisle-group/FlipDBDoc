# gt

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Relational{.info}

Greater than.{.purpose}

~~~
R=X gt Y
~~~

X and Y must be numeric. R is boolean and is 1 if X is greater than Y. gt may also be written as >.

## Examples

~~~
       3 > 1 2 3 4
┌───────┐
↓1      │
│1      │
│0      │
│0      │
└Boolean┘
      a=1 2 3 4 5
      b=5 4 3 2 1
      a gt b
┌───────┐
↓0      │
│0      │
│0      │
│1      │
│1      │
└Boolean┘
      b > a
┌───────┐
↓1      │
│1      │
│0      │
│0      │
│0      │
└Boolean┘
~~~

