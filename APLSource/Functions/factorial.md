# factorial

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=factorial X
~~~

Y must be numeric excluding negative integers. R is numeric.

R is the product of the first Y integers for positive integer values of Y. For non-integral values
of Y, Y is equivalent to the gamma function of Y+1.

## Examples

~~~
      factorial 1 2 3 4 5
┌───────────────────┐
↓  1                │
│  2                │
│  6                │
│ 23.999999999999993│
│119.99999999999997 │
└Float──────────────┘
      factorial -1.5 0 1.5 3.3
┌───────────────────┐
↓-3.544907701811028 │
│ 1                 │
│ 1.3293403881791355│
│ 8.855343360454029 │
└Float──────────────┘
~~~

