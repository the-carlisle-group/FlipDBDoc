# minus

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=X minus Y
~~~

X and Y must be numeric. R is the difference between X and Y.  R is numeric. This function may also
be written symbolically as '-'.

Note that the minus symbol is also used to denote a negative number, and thus when used as a
function, it must be followed by a space.

## Examples

~~~
       10-4
┌────┐
↓10  │
│(4) │
└Int8┘
       10 minus 4
┌────┐
│6   │
└Int8┘
       10 - 4 12
┌────┐
↓6   │
│(2) │
└Int8┘
      x=100 2 3
      y=1 20 30
      x minus y
┌────┐
↓99  │
│(18)│
│(27)│
└Int8┘
~~~

