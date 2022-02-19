# pick

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Selection{.info}

Select an item by index.{.purpose}

~~~
R=X pick Y
~~~

X is zero, one or two →integers. Y is any column or object array. R is one of:

* An element of Y yielding a scalar
* A disclosed partition of Y yielding a simple column
* An object of Y
* All of Y

Zero (0) picks the first or only item of a structure with subsequent items being picked by
increasingly higher integers. If X is empty, R is all of Y. If X is a single number (n), R is the
nth element of simple Y, the disclosed contents of the nth partition of partitioned Y or the nth
object of array Y. If X is two numbers (m,n), they pick the nth item of the mth partition of
partitioned Y.

See also: →[index]

## Examples

~~~
      0 pick 3 2 1
┌────┐
│3   │
└Int8┘
      4 pick 'Tim,Len,Jon,Pete,Harry,Mike'
┌───────┐
│Harry  │
└Char(5)┘
      a= enclose (1 2 3) (4 5 6 7) (8 9)
      a
┌─────────┐
↓[1,2,3]  │
│[4,5,6,7]│
│[8,9]    │
└Int8─────┘
      2 pick a
┌────┐
↓8   │
│9   │
└Int8┘
~~~

