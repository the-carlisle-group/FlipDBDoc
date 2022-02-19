# toNumeric

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=toNumeric X
~~~

Converts column X to numeric. R is the lowest (smallest) possible type. R is 0 where X does not
convert to a valid number.

Although the main purpose of toNumeric is to convert a Char column into a numeric one, X can be
of any type but Blob.

Note that any lines which contain digits are likely to produce a number, because all non-digits but
these are removed: `.-¯`

## Examples

~~~
       toNumeric '0,1'
┌───────┐
↓0      │
│1      │
└Boolean┘
       toNumeric '0,1,2'
┌────┐
↓0   │
│1   │
│2   │
└Int8┘
      toNumeric '0,1,2,3.41'
┌──────┐
↓0.00  │
│1.00  │
│2.00  │
│3.41  │
└Dec(2)┘
~~~

