# progressiveIndexOf

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Selection{.info}

find, locate, where{.purpose}

~~~
R=X progressiveIndexOf Y
~~~

R is Integer being the positions in which the items of Y
are found in X. Once an item in Y has been found in X, it
is no longer available for subsequnet items in Y to match.
Thus items in X are "used up" as they are found. This is in
distinction to →[indexOf] which will match all equal items in Y
to the first occurance in X.

See also: →[indexOf] →[lookup]

## Examples:

~~~
      'A,A,B,C,D'  progressiveIndexOf 'A,A,A,B,C,D,E'
┌────┐
↓0   │
│1   │
│5   │
│2   │
│3   │
│4   │
│5   │
└Int8┘

      'A,A,B,C,D'  indexOf 'A,A,A,B,C,D,E'
┌────┐
↓0   │
│0   │
│0   │
│2   │
│3   │
│4   │
│5   │
└Int8┘
~~~

