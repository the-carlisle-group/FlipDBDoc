# in

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Relational{.info}

Membership{.purpose}

~~~
R=X in Y
~~~

X and Y must be compatible types. R is boolean being 1 for each element in X that is also an
element of Y or of any of its partitions and 0 otherwise.

## Examples

~~~
      a='NY,NY,CA,NJ,PA'
      a
┌───────┐
↓NY     │
│NY     │
│CA     │
│NJ     │
│PA     │
└Char(2)┘
      a in 'NY,NJ'
┌───────┐
↓1      │
│1      │
│0      │
│1      │
│0      │
└Boolean┘
      5 in 3 4 5
┌───────┐
│1      │
└Boolean┘
~~~

