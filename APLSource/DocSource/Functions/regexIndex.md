# regexIndex

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Find the starting position of regular expresssion matches.{.purpose}

~~~
R=[M] regexIndex X Y
~~~

X is a character column. Y is one or more search patterns.
M is an optional match limit.
R is Integer, the starting position of matches found in X.

If M is 0, the default, there is no limit. Positive N means
match up to N times. Negative N means match only the Nth match.

If no match is found, and the structure of R requires a value,
the length of the subject string is returned.

See also →[regexSearch] →[regexReplace] →[regexMatch] →[regexLength] →[regexSplit]

## Examples

~~~

      regexIndex 'Mississippi' 'i..'
┌────┐
↓1   │
│4   │
│7   │
└Int8┘

      1 regexIndex 'Mississippi' 'i..'
┌────┐
│1   │
└Int8┘

      regexIndex 'Mississippi' 'NoSuchMatch'
┌────┐
↓Int8┘
      1 regexIndex 'Mississippi' 'NoSuchMatch'
┌────┐
│11  │
└Int8┘

      State=  'Mississippi,Californa,New York'

      State (regexIndex State ('or.' 'i.'))
 ┌───────────┐  ┌───────┐
 ↓Mississippi│  ↓[1,4,7]│
 │Californa  │  │[3,5]  │
 │New York   │  │[5]    │
 └Char(11)───┘  └Int8───┘

      State ( -3 regexIndex State ('or.' 'i.'))
 ┌───────────┐  ┌────┐
 ↓Mississippi│  ↓7   │
 │Californa  │  │9   │
 │New York   │  │8   │
 └Char(11)───┘  └Int8┘

~~~
