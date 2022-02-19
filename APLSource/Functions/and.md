# and

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Logical{.info}

Logical and, both{.purpose}

~~~
R=X and Y
~~~

X and Y must both be boolean. R is Boolean being 1 if both X & Y are 1.

See also: →[all]

## Examples

~~~
      B3 = 0 0 1 1
      B5 = 0 1 0 1
      B3 B5 (B3 and B5)
 ┌───────┐  ┌───────┐  ┌───────┐
 ↓0      │  ↓0      │  ↓0      │
 │0      │  │1      │  │0      │
 │1      │  │0      │  │0      │
 │1      │  │1      │  │1      │
 └Boolean┘  └Boolean┘  └Boolean┘
~~~

