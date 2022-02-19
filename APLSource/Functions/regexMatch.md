# regexMatch

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Find and extract regular expression matches.{.purpose}

~~~
R=[M] regexMatch X Y
~~~

X is a character column. Y is one or more search patterns.
M is an optional match limit which defaults to 0.
R is character, containing the matches found in X.

If M is 0, the default, there is no limit. Positive N means
match up to N times. Negative N means match only the Nth match.

See also →[regexSearch] →[regexReplace] →[regexLength] →[regexIndex] →[regexSplit]

## Examples

~~~
      regexMatch 'Find the words in this sentence' 'w+'
┌────────┐
↓Find    │
│the     │
│words   │
│in      │
│this    │
│sentence│
└Char(8)─┘

      -3 regexMatch 'Extract the third word' 'w+'
┌───────┐
│third  │
└Char(5)┘

      State='Mississippi,California,New York'

      State (regexMatch State 'i..')
 ┌───────────┐  ┌─────────────┐
 ↓Mississippi│  ↓[iss,iss,ipp]│
 │California │  │[ifo]        │
 │New York   │  │[]           │
 └Char(11)───┘  └Char(3)──────┘

~~~
