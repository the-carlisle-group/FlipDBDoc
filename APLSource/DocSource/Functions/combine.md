# combine

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Combines the corresponding items of multiple columns.{.purpose}

~~~
R=combine X
~~~

X is an array of two or more columns of compatible type and equal shape,
R is single a partitioned column
where each item is composed of the corresponding items of each column in
X laminated together.

This function is equivalent to:

~~~
R=append each over X
~~~

## Examples

~~~
      a=1 2 3 4 5
      b=6 7 8 9 10
      c=11 12 13 14 15
      a b c
 ┌────┐  ┌────┐  ┌────┐
 ↓1   │  ↓6   │  ↓11  │
 │2   │  │7   │  │12  │
 │3   │  │8   │  │13  │
 │4   │  │9   │  │14  │
 │5   │  │10  │  │15  │
 └Int8┘  └Int8┘  └Int8┘
      combine a b c
┌─────────┐
↓[1,6,11] │
│[2,7,12] │
│[3,8,13] │
│[4,9,14] │
│[5,10,15]│
└Int8─────┘
      d=enclose a b c
      e=100 200 300
      f=enclose (1 2) (1 2 3) (0)
      d e f
 ┌────────────────┐  ┌─────┐  ┌───────┐
 ↓[1,2,3,4,5]     │  ↓100  │  ↓[1,2]  │
 │[6,7,8,9,10]    │  │200  │  │[1,2,3]│
 │[11,12,13,14,15]│  │300  │  │[0]    │
 └Int8────────────┘  └Int16┘  └Int8───┘
      combine d e f
┌──────────────────────┐
↓[1,2,3,4,5,100,1,2]   │
│[6,7,8,9,10,200,1,2,3]│
│[11,12,13,14,15,300,0]│
└Int16─────────────────┘
~~~

