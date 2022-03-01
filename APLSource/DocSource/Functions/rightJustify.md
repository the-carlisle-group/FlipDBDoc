# rightJustify

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Shifts all characters to the right.{.purpose}

~~~
R=rightJustify X
~~~

X must be Char. R is Char. R is X with trailing blanks removed, and padded with leading blanks to
maintain width.

## Examples

~~~
      T='zero,one,twohundred'
      T
┌──────────┐
↓zero      │
│one       │
│twohundred│
└Char(10)──┘
      rightJustify T
┌──────────┐
↓      zero│
│       one│
│twohundred│
└Char(10)──┘
~~~

