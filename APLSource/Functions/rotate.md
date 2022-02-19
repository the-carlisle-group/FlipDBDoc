# rotate

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

rotate, shift, offset{.purpose}

~~~
R=X rotate Y
~~~

X is an Integer. Y is any column or object array. R is Y with its elements, partitions or objects
rotated by X positions.

If X is positive, the leading X items are moved to the end; if negative, the trailing X items are
moved to the start; if zero, the items remain unmoved. If →[absoluteValue] X is greater than the
→[shape] of Y, items are shifted by (shape Y) →[residue] X.

## Examples:

~~~
       1 rotate 1 2 3 4 5
┌────┐
↓2   │
│3   │
│4   │
│5   │
│1   │
└Int8┘
        -2 rotate 'A,B,C,D,E'
┌───────┐
↓D      │
│E      │
│A      │
│B      │
│C      │
└Char(1)┘
~~~

