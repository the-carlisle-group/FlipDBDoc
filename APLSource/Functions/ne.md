# ne

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Relational{.info}

Not equal to.{.purpose}

~~~
R=X ne Y
~~~

X and Y must be conforming types. R is boolean. R is 1 if X is not equal to Y. ne may also be
written as '<>'.

## Examples

~~~
      1 ne 0 1 2
┌───────┐
↓1      │
│0      │
│1      │
└Boolean┘
      'Tom' <> 'Jon,Tom,Tim,Tom'
┌───────┐
↓1      │
│0      │
│1      │
│0      │
└Boolean┘
~~~

