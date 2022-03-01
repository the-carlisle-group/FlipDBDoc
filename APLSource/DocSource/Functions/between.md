# between

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Relational{.info}

Membership{.purpose}

~~~
R=X between A B
~~~

A is the lower bound and B is the upper bound. R is boolean being 1 for each element in X that is
also between A and B exclusive. This is equivalent to:

~~~
(X > A) and (X < B)
~~~

## Examples

~~~
      1 2 3 4 5 between 2 4
┌───────┐
↓0      │
│0      │
│1      │
│0      │
│0      │
└Boolean┘
      (enclose 1 2 3 4 5) between 2 4
┌───────────┐
│[0,0,1,0,0]│
└Boolean────┘
      (enclose(1 2 3)(4 5))between 2 4
┌───────┐
↓[0,0,1]│
│[0,0]  │
└Boolean┘
~~~

