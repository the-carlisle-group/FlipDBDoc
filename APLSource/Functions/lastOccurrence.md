# lastOccurrence

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Relational{.info}

Flags the last occurrence of an item.{.purpose}

~~~
R=lastOccurrence X
~~~

X can be any data type but Blob. R is boolean. Items in R get 1 where the corresponding item in X
occurred for the last time.

## Examples

~~~
      V=1 2 3
      lastOccurrence V
┌───────┐
↓1      │
│1      │
│1      │
└Boolean┘
      V=1 2 1 2 3 1 1 1
      lastOccurrence V
┌───────┐
↓0      │
│0      │
│0      │
│1      │
│1      │
│0      │
│0      │
│1      │
└Boolean┘
      N='Tim,John,Tim,John'
      lastOccurrence N
┌───────┐
↓0      │
│0      │
│1      │
│1      │
└Boolean┘
~~~

