# at

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Replace an indexed item{.purpose}

~~~
R=X at I Z
~~~

X is a simple column.  Note that with does not work on object arrays.

I is a scalar Integer that indexes X.

Z is a scalar item of a type compatible with X.

R is the items of X with the Ith item replaced by Z.

The type of R is determined from those of X & Z such that no precision will be lost.

## Examples

~~~
     10 11 12 13 at 2 99
┌────┐
↓10  │
│11  │
│99  │
│13  │
└Int8┘
      'A,B,C,D,E' at 4 'This was E'
┌──────────┐
↓A         │
│B         │
│C         │
│D         │
│This was E│
└Char(10)──┘
~~~

