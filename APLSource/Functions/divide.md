# divide

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=X divide Y
~~~

X and Y must be numeric.  R is X divided by Y. R is Float.

The result of any number divided by 0 is 0. The divide function may also be written as '`/`'.

## Examples

~~~
      a=1 2 3
      b=0 1 -2
      a b
 ┌────┐  ┌────┐
 ↓1   │  ↓0   │
 │2   │  │1   │
 │3   │  │(2) │
 └Int8┘  └Int8┘
      a divide b
┌─────┐
↓ 0   │
│ 2   │
│-1.5 │
└Float┘
      b / a
┌─────────────┐
↓ 0           │
│ 0.5         │
│-0.6666666667│
└Float────────┘
~~~

