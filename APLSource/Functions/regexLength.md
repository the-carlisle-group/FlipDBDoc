# regexLength

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Find the length of regular expression matches.{.purpose}

~~~
R=[M] regexLength X Y
~~~

X is a character column. Y is one or more search patterns. M is an optional the match limit that defaults to 0.
R is Integer, the length of matches found in X.

If M is 0, the default, there is no limit. Positive N means
match up to N times. Negative N means match only the Nth match.

See also →[regexSearch] →[regexReplace] →[regexMatch] →[regexIndex] →[regexSplit]

## Examples

~~~

       regexLength 'Count the word lengths in this sentence' 'w+'
┌────┐
↓5   │
│3   │
│4   │
│7   │
│2   │
│4   │
│8   │
└Int8┘

       PayHistory='000000000000,001222222222,000122012222,01234FFFFFFF'
       PayHistory (regexLength PayHistory '2+')
 ┌────────────┐  ┌─────┐
 ↓000000000000│  ↓[]   │
 │001222222222│  │[9]  │
 │000122012222│  │[2,4]│
 │01234FFFFFFF│  │[1]  │
 └Char(12)────┘  └Int8─┘

~~~
