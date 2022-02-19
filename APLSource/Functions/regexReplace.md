# regexReplace

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Search and replace text with regular expressions.{.purpose}

~~~
R=[M] regexReplace X Y Z
~~~

X is a character column. Y is one or more search patterns.
Z is one or more replacement patterns.
M is an optional match limit.
R is X with matches indicated by Y replaced with Z.

If M is 0, the default, there is no limit. Positive N means
match up to N times. Negative N means match only the Nth match.

See also →[regexSearch] →[regexMatch] →[regexLength] →[regexIndex] →[regexSplit]

## Examples

~~~
      regexReplace 'Billy the Kid' 'Billy' 'William'
┌───────────────┐
│William the Kid│
└Char(15)───────┘

      regexReplace 'Put quotes around each word' 'w+' '"&"'
┌─────────────────────────────────────┐
│"Put" "quotes" "around" "each" "word"│
└Char(37)─────────────────────────────┘

~~~

