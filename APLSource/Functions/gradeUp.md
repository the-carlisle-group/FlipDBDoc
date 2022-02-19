# gradeUp

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Sorting{.info}

Index for an ascending sort{.purpose}

~~~
R=gradeUp X
~~~

X can be any column but Blob. R is integer being the ascending sort order of X.

## Examples

~~~
      L='Aa,Ac,Ab,Arg,Ärger,ärgerlich,angst'
      (gradeUp L) index L
┌─────────┐
↓Aa       │
│Ab       │
│Ac       │
│Arg      │
│angst    │
│Ärger    │
│ärgerlich│
└Char(9)──┘
      L=1 2 8 9 3 4 -3
      (gradeUp L) index L
┌────┐
↓-3  │
│ 1  │
│ 2  │
│ 3  │
│ 4  │
│ 8  │
│ 9  │
└Int8┘
~~~

See also: →[gradeDown], →[sortDown], →[sortUp]

