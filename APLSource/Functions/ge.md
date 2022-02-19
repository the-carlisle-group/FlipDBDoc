# ge

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Relational{.info}

Greater than or equal to.{.purpose}

~~~
R=X ge Y
~~~

X and Y must be numeric. R is boolean and is 1 if X is greater than or equal to Y. This function
may also be written as `>=`.

## Examples

~~~
      3 ge 1 2 3 4 5 6
┌───────┐
↓1      │
│1      │
│1      │
│0      │
│0      │
│0      │
└Boolean┘
       a=1 2 3 4 5
       b=5 4 3 2 1
       a >= b
┌───────┐
↓0      │
│0      │
│1      │
│1      │
│1      │
└Boolean┘
      b ge a
┌───────┐
↓1      │
│1      │
│1      │
│0      │
│0      │
└Boolean┘
~~~

