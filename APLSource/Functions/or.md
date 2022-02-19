# or

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Logical{.info}

Logical or.{.purpose}

~~~
R=X or Y
~~~

X and Y must both be boolean. R is Boolean being 1 if either X or Y is 1.

See also: →[any]

## Examples

~~~
      B3 = 0 0 1 1
      B5 = 0 1 0 1
      B3 B5 (B3 or B5)
 ┌───────┐  ┌───────┐  ┌───────┐
 ↓0      │  ↓0      │  ↓0      │
 │0      │  │1      │  │1      │
 │1      │  │0      │  │1      │
 │1      │  │1      │  │1      │
 └Boolean┘  └Boolean┘  └Boolean┘
~~~

