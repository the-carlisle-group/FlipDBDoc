# window

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

~~~
R=X window Y
~~~

R is Y with its items replicated and partitioned according to the window defined by X.  Y must be a
simple column and the result R contains a partitioned column of the same type and length as Y.

The window X is specified by one, two, or three values. If only one value is provided, it is
interpreted as the window size, with the window ending at the current item. If two items are
provided, the first is interpreted as the offset from the current row where the window starts, and
the second is the offset from the current row where the window ends. The current row is indicated
by a zero. Negative offsets indicate a preceding row, positive offsets a succeeding row. Thus the
window (3) is equivalent to (-2 0). The third item is an optional flag which indicates whether the
current row is to be included (1) or excluded (0) from each window. The default is 1.

If the first offset is greater than the second they are treated as reversed but the resulting
partitions are themselves reversed.

Some leading or trailing partitions may be shorter than the rest. If both offsets are negative or
both positive then one or more will be empty.

## Examples

~~~
      A=integers 10
      A (0 2 window A) (-1 1 window A) (-5 -2 window A)
 ┌────┐  ┌───────┐  ┌───────┐  ┌─────────┐
 ↓0   │  ↓[0,1,2]│  ↓[0,1]  │  ↓[]       │
 │1   │  │[1,2,3]│  │[0,1,2]│  │[]       │
 │2   │  │[2,3,4]│  │[1,2,3]│  │[0]      │
 │3   │  │[3,4,5]│  │[2,3,4]│  │[0,1]    │
 │4   │  │[4,5,6]│  │[3,4,5]│  │[0,1,2]  │
 │5   │  │[5,6,7]│  │[4,5,6]│  │[0,1,2,3]│
 │6   │  │[6,7,8]│  │[5,6,7]│  │[1,2,3,4]│
 │7   │  │[7,8,9]│  │[6,7,8]│  │[2,3,4,5]│
 │8   │  │[8,9]  │  │[7,8,9]│  │[3,4,5,6]│
 │9   │  │[9]    │  │[8,9]  │  │[4,5,6,7]│
 └Int8┘  └Int8───┘  └Int8───┘  └Int8─────┘
~~~

The combination of an aggregate function with the window function operates like a hybrid function:

~~~
      A (3 window A) (sum 3 window A) (average 3 window A)
 ┌────┐  ┌───────┐  ┌────┐  ┌─────┐
 ↓0   │  ↓[0]    │  ↓ 0  │  ↓0.0  │
 │1   │  │[0,1]  │  │ 1  │  │0.5  │
 │2   │  │[0,1,2]│  │ 3  │  │1.0  │
 │3   │  │[1,2,3]│  │ 6  │  │2.0  │
 │4   │  │[2,3,4]│  │ 9  │  │3.0  │
 │5   │  │[3,4,5]│  │12  │  │4.0  │
 │6   │  │[4,5,6]│  │15  │  │5.0  │
 │7   │  │[5,6,7]│  │18  │  │6.0  │
 │8   │  │[6,7,8]│  │21  │  │7.0  │
 │9   │  │[7,8,9]│  │24  │  │8.0  │
 └Int8┘  └Int8───┘  └Int8┘  └Float┘
~~~

The "scan" family of functions operate as if they had a built-in window of all preceding items to
the current item (An equivalent scan function will be significantly faster and more efficient than
using window):

~~~
      A  (plusScan A) (sum lowestInteger 0 window A)
 ┌────┐  ┌────┐  ┌────┐
 ↓0   │  ↓ 0  │  ↓ 0  │
 │1   │  │ 1  │  │ 1  │
 │2   │  │ 3  │  │ 3  │
 │3   │  │ 6  │  │ 6  │
 │4   │  │10  │  │10  │
 │5   │  │15  │  │15  │
 │6   │  │21  │  │21  │
 │7   │  │28  │  │28  │
 │8   │  │36  │  │36  │
 │9   │  │45  │  │45  │
 └Int8┘  └Int8┘  └Int8┘
~~~

The window function exhibits properties of both a structural function (it returns a partitioned
column from a simple column), and a hybrid function (it always returns a column with the same
number of items as its argument). Because of this, it may be used with the partitionBy operator,
even though its argument must be simple, and cannot be called directly on a partitioned column:

~~~
      G=10 reshape 'Red,Blue'
      A G (3 window A) (3 window partitionBy A G)
 ┌────┐  ┌───────┐  ┌───────┐  ┌───────┐
 ↓0   │  ↓Red    │  ↓[0]    │  ↓[0]    │
 │1   │  │Blue   │  │[0,1]  │  │[1]    │
 │2   │  │Red    │  │[0,1,2]│  │[0,2]  │
 │3   │  │Blue   │  │[1,2,3]│  │[1,3]  │
 │4   │  │Red    │  │[2,3,4]│  │[0,2,4]│
 │5   │  │Blue   │  │[3,4,5]│  │[1,3,5]│
 │6   │  │Red    │  │[4,5,6]│  │[2,4,6]│
 │7   │  │Blue   │  │[5,6,7]│  │[3,5,7]│
 │8   │  │Red    │  │[6,7,8]│  │[4,6,8]│
 │9   │  │Blue   │  │[7,8,9]│  │[5,7,9]│
 └Int8┘  └Char(4)┘  └Int8───┘  └Int8───┘
~~~

