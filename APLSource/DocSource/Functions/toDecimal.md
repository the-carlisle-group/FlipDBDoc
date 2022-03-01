# toDecimal

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=[X] toDecimal Y
~~~

Converts column Y to Decimal with scale X. Y may be any type except Blob. X must be a literal
integer from 1 to 9. It defaults to 2. Y will be truncated at the specified number of decimal
places. The →[round] and →[toNumeric] functions may be used to obtain a decimal type by rounding
rather than truncation:

~~~
R=X round toNumeric Y
~~~

Note that any lines which contain digits are likely to produce a number, because all non-digits but
these: `.-¯` are removed.

## Examples

~~~
      toDecimal '5.879'
┌──────┐
│5.87  │
└Dec(2)┘
      3 toDecimal '5.879'
┌──────┐
│5.879 │
└Dec(3)┘
      1 toDecimal '5.879'
┌──────┐
│5.8   │
└Dec(1)┘
      5 toDecimal '5.879'
┌───────┐
│5.87900│
└Dec(5)─┘
~~~

