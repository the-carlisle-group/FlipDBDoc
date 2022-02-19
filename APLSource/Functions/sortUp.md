# sortUp

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Sorting{.info}

~~~
R=sortUp X
~~~

X is a column of any data type but Blob.

R is X with its items sorted in ascending order.

If X is Char then the ordering is by Unicode code point.

See also: →[sortDown], →[gradeDown], →[gradeUp]

## Examples

~~~
      B=10 2 5 9 7 8 1 3 4 6
      sortUp B
┌────┐
↓ 1  │
│ 2  │
│ 3  │
│ 4  │
│ 5  │
│ 6  │
│ 7  │
│ 8  │
│ 9  │
│10  │
└Int8┘
~~~

