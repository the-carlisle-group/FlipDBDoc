# product

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Arithmetic{.info}

Return the product or multiple of a list of numbers.{.purpose}

~~~
R=product X
~~~

X must be numeric.  R is numeric and is the product of X, the number produced by multiplying all
the elements of X together.

## Examples

~~~
     product 3 4 5
┌────┐
│60  │
└Int8┘
      product emptyInteger
┌────┐
│1   │
└Int8┘
      product 5
┌────┐
│5   │
└Int8┘
~~~

