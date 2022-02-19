# lookup

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Selection{.info}

Look up elements.{.purpose}

~~~
R=X lookup Y
~~~

X and Y are arrays of one or more columns, where each corresponding column in X
and Y is of the same type. X and Y may be thought of as key columns.
Each column in X must be simple and have the same shape.
Each column Y must have the same shape and structure.
R is Integer being the positions in which the combined items of Y
are found in the combined items of X the first time.

If Y is a single, simple column, then this function is equivalent to →[indexOf].

See also →[indexOf] →[progressiveIndexOf]

## Examples:

~~~
      'NY,NY,CT,NJ' 'A,D,B,C' lookup 'NY,CT,NJ,NY,NY' 'A,B,C,D,E'
┌────┐
↓0   │
│2   │
│3   │
│1   │
│4   │
└Int8┘




~~~

