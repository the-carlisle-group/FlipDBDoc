# gradeDown

Type:{.prefix}

Hybrid{.info}

Categories:{.prefix}

Sorting{.info}

Index for a descending sort{.purpose}

~~~
R=gradeDown X
~~~

X can be any column but Blob. R is integer being the descending sort order of X.

## Examples

~~~
      L='Aa,Ac,Ab,Arg,Ärger,ärgerlich,angst'
      (gradeDown L) index L
┌─────────┐
↓ärgerlich│
│Ärger    │
│angst    │
│Arg      │
│Ac       │
│Ab       │
│Aa       │
└Char(9)──┘
      L=1 2 8 9 3 4 -3
      (gradeDown L) index L
┌────┐
↓ 9  │
│ 8  │
│ 4  │
│ 3  │
│ 2  │
│ 1  │
│-3  │
└Int8┘
~~~

See also: →[gradeUp], →[sortDown], →[sortUp]

