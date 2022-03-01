# signum

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

Sign of number.

~~~
R=signum X
~~~

X must be numeric. R is 0 if X is 0, 1 if X is positive, and -1 if X is negative.

## Examples

~~~
      signum -2 -1 0 1 2
┌────┐
↓-1  │
│-1  │
│ 0  │
│ 1  │
│ 1  │
└Int8┘
~~~

