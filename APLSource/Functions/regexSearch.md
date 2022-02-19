# regexSearch

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Regular expression search.{.purpose}

~~~
R=regexSearch X Y
~~~

X is a character column. Y is one or more search patterns. R is boolean indicating where Y is found in X.

See also →[regexReplace] →[regexMatch] →[regexLength] →[regexIndex] →[regexSplit]

## Examples

~~~

      regexSearch 'Mississippi' 'issi'
┌───────┐
│1      │
└Boolean┘

      regexSearch 'Mississippi,California,New York' 'i..'
┌───────┐
↓1      │
│1      │
│0      │
└Boolean┘

~~~
