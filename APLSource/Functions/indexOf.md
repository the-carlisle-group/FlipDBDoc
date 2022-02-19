# indexOf

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Selection{.info}

find, locate, where{.purpose}

~~~
R=X indexOf Y
~~~

X is any non-scalar column. Y is any column.
R is Integer being the positions in which the items of Y are found in X the first time.

If either X or Y is an array of columns, then both X and Y are treated as
an array of one or more columns, and we are locating entire colums, not
elements or partitions.

For items of Y found in X the values of R will range from zero for the first position to
N-1 for the last, where N is the →[shape] of X.
Items of Y not found in X will have N as their
corresponding item in R.

For multi-column lookups, see →[lookup].

See also →[progressiveIndexOf]

## Examples:

~~~
      a='CA,NY,NJ,CT,NY,NY,CA'
      b='CA,NJ,NY'
      b indexOf a
┌────┐
↓0   │
│2   │
│1   │
│3   │
│2   │
│2   │
│0   │
└Int8┘
      a indexOf b
┌────┐
↓0   │
│2   │
│1   │
└Int8┘
~~~

