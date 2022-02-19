# leftJustify

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Remove leading blanks.{.purpose}

~~~
R=leftJustify X
~~~

X must be Char. R is X with leading blanks removed, and padded with blanks to maintain width.  R is Character.

## Examples

~~~
      T = 'zero, one,  two,   three,    four'
      T (leftJustify T)
 ┌────────┐  ┌────────┐
 ↓zero    │  ↓zero    │
 │ one    │  │one     │
 │  two   │  │two     │
 │   three│  │three   │
 │    four│  │four    │
 └Char(8)─┘  └Char(8)─┘
~~~

