# regexSplit

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Partition a string based on regular expression matches.{.purpose}

~~~
R=[M] regexSplit X Y
~~~

X is a character column. Y is one or more search patterns.
M is an optional match limit. The default is 0, for match all.
R is X partitioned according to matches specified by Y.

If M is 0, the default, there is no limit. Positive N means
match up to N times. Negative N means match only the Nth match.

See also →[regexSearch] →[regexReplace] →[regexMatch] →[regexLength] →[regexIndex] →[split] →[splitCSV]

## Examples
