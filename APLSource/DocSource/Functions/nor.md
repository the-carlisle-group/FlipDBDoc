# nor

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Logical{.info}

Logical nor.{.purpose}

~~~
Z=X nor Y
~~~

X and Y must be boolean. Z is Boolean being 1 if both X & Y are 0.

## Examples

~~~
      B3 = 0 0 1 1
      B5 = 0 1 0 1
      B3 B5 (B3 nor B5)
 ┌───────┐  ┌───────┐  ┌───────┐
 ↓0      │  ↓0      │  ↓1      │
 │0      │  │1      │  │0      │
 │1      │  │0      │  │0      │
 │1      │  │1      │  │0      │
 └Boolean┘  └Boolean┘  └Boolean┘
~~~

