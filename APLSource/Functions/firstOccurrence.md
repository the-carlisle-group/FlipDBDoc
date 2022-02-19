# firstOccurrence

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Relational{.info}

Flags the first occurrence of each item in a column.{.purpose}

~~~
R=firstOccurrence X
~~~

X can be any data type but Blob. R is boolean. Items in R get 1 where the corresponding item in X
occurred for the first time.

## Examples

~~~
      firstOccurrence 1 2 3
┌───────┐
↓1      │
│1      │
│1      │
└Boolean┘
      firstOccurrence 1 2 1 2 3 3
┌───────┐
↓1      │
│1      │
│0      │
│0      │
│1      │
│0      │
└Boolean┘
      firstOccurrence 'A,B,B,A,A,A'
┌───────┐
↓1      │
│1      │
│0      │
│0      │
│0      │
│0      │
└Boolean┘
~~~

