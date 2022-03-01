# residue

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=X residue Y
~~~

X and Y must be numeric. R is the remainder of Y divided by X.

For positive arguments, R is the remainder when Y is divided by X. If X is 0, R is Y. For other
argument values, R is Y minus N times X where N is some integer such that R lies between 0 and
X, but is not equal to X.

## Examples

~~~
      3 3 -3 -3 residue -5 5 -4 4
┌─────┐
↓ 1   │
│ 2   │
│-1   │
│-2   │
└Float┘
      0.5 residue 3.12 -1 -0.6
┌──────────────────┐
↓0.1200000000000001│
│0                 │
│0.4               │
└Float─────────────┘
      -1 0 1 residue -5.25 0 2.41
┌────────────────────┐
↓-0.25               │
│ 0                  │
│ 0.41000000000000014│
└Float───────────────┘
~~~

