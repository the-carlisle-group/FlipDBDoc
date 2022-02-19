# length

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Length of a string.{.purpose}

~~~
R=length X
~~~

X must be Char. R is Integer being the number of characters excluding trailing blanks in each item
of X.

See the structural function →shape for counting the number of items in a column, rather than the
number of characters in a string.

## Examples

~~~
      length ''
┌────┐
│0   │
└Int8┘
      length 'Hello world'
┌────┐
│11  │
└Int8┘
      length 'one,two,three,four'
┌────┐
↓3   │
│3   │
│5   │
│4   │
└Int8┘
~~~

