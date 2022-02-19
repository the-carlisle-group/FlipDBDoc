# sortDown

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Sorting{.info}

~~~
R=sortDown X
~~~

X is a column of any data type but Blob.

R is X with its items sorted in descending order.

If X is Char then the ordering is by Unicode code point.

See also: →[sortUp], →[gradeDown], →[gradeUp]

## Examples

~~~
      B=10 2 5 9 7 8 1 3 4 6
      sortDown B
┌────┐
↓10  │
│ 9  │
│ 8  │
│ 7  │
│ 6  │
│ 5  │
│ 4  │
│ 3  │
│ 2  │
│ 1  │
└Int8┘
~~~

