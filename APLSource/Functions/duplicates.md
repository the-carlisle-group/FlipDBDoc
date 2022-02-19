# duplicates

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Flag the duplicates in a column{.purpose}

~~~
R=duplicates X
~~~

X can be any column but Blob. R is boolean marking values
in X that occur more than once in X.

It is equivalent to:

~~~
(occurences X) gt 1
~~~

See also →[firstOccurrence], →[occurrences]

## Examples

~~~
      duplicates 5 6 7 5 8 9 5 6
┌───────┐
↓1      │
│1      │
│0      │
│1      │
│0      │
│0      │
│1      │
│1      │
└Boolean┘
~~~

