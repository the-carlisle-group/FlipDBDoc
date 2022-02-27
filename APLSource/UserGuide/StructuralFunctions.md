# Structural Functions

Structural functions select, generate, replicate and re-arrange data. They often produce a result
that is a different shape or structure from the primary argument. For example the →[*.take]
function, with a positive left argument, extracts items from the top of a column:

~~~
      3 take 5 6 7 8 9
┌────┐
↓5   │
│6   │
│7   │
└Int8┘
~~~

The [integers]() function generates integers from 0 up to 1 less than its argument:

~~~
      integers 5
┌────┐
↓0   │
│1   │
│2   │
│3   │
│4   │
└Int8┘
~~~

The [replicate]() function repeats the items in its right argument according to the
corresponding integers of its left argument:

~~~
      1 2 3 replicate 'A,B,C'
┌───────┐
↓A      │
│B      │
│B      │
│C      │
│C      │
│C      │
└Char(1)┘
~~~

And the [append]() function appends the items of its right argument to the items of its left argument:

~~~
      1 2 3 append 1.1 2.2
┌──────┐
↓1.0   │
│2.0   │
│3.0   │
│1.1   │
│2.2   │
└Dec(1)┘
~~~

When applied to a partitioned column, structural functions operate on the column in its entirety
rather than each item individual and independently. This is in marked contrast to aggregate and
hybrid functions. Let's create a partitioned column to see how this works:

~~~
      a=enclose  (1 2 3) (4 5) (6 7 8 9)
      a
┌─────────┐
↓[1,2,3]  │
│[4,5]    │
│[6,7,8,9]│
└Int8─────┘
~~~

We can use the take function to extract the last 2 items from this column:

~~~
      -2 take a
┌─────────┐
↓[4,5]    │
│[6,7,8,9]│
└Int8─────┘
~~~

In order to apply the take function to the items of a partitioned column individually, we need to
use the →[*.each] operator:

~~~
      -2 take each a
┌─────┐
↓[2,3]│
│[4,5]│
│[8,9]│
└Int8─┘
~~~

FlipDB provides a rich collection of structural functions for manipulating character, numeric, and
temporal data.


