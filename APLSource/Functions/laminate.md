# laminate

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Juxtapose columns.{.purpose}

~~~
R=X laminate Y
~~~

X and Y must be conformable Char columns. R is X and Y juxtaposed horizontally. Unlike →[catenate]
which chains a string in one column after the last non-blank of the corresponding string in the
previous column, laminate aligns all strings in Y after the full width of corresponding strings in X.

## Examples

~~~
      'one     ' laminate 'two'
┌───────────┐
│one     two│
└Char(11)───┘
       'One,two,three,seven' laminate ' abc'
┌─────────┐
↓One   abc│
│two   abc│
│three abc│
│seven abc│
└Char(9)──┘
~~~

