# subString

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text,Selection{.info}

Part of a string.{.purpose}

~~~
R=X subString Y
~~~

Y is Char.

X is 1 or 2 integers: +/- offset [length]

R is Char and of the same structure as Y.

Offset is counted rightwards starting with zero at the left hand side of the text or leftwards
starting with minus one at the last non-blank on each row.

If length is supplied, up to length characters are returned for each row of Y starting at offset,
negative length being treated as zero. Otherwise, everything from offset rightwards is returned.

## Examples

~~~
    3 5 subString 'Hello World'
┌───────┐
│lo Wo  │
└Char(5)┘
~~~

