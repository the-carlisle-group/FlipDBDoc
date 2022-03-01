# nand

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Logical{.info}

Logical nand, not both{.purpose}

~~~
Z=X nand Y
~~~

X and Y must be boolean. Z is Boolean being 1 if either of X & Y is zero.

## Examples

~~~
      B3 = 0 0 1 1
      B5 = 0 1 0 1
      B3 B5 (B3 nand B5)
 ┌───────┐  ┌───────┐  ┌───────┐
 ↓0      │  ↓0      │  ↓1      │
 │0      │  │1      │  │1      │
 │1      │  │0      │  │1      │
 │1      │  │1      │  │0      │
 └Boolean┘  └Boolean┘  └Boolean┘
~~~

