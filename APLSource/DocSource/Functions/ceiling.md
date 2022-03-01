# ceiling

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=ceiling X
~~~

X must be numeric. R is the lowest integer not less than X.  R is Integer.

## Examples

~~~
       ceiling 1 1.2 3.9999 -3.4
┌────┐
↓ 1  │
│ 2  │
│ 4  │
│-3  │
└Int8┘
~~~

