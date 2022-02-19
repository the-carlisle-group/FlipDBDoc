# group

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Compute the corresponding indices for each unique value in a column.{.purpose}

~~~
R=[X] group Y
~~~

Y is one or more simple DataColumns of the same length, of any type except Float or Blob. X is zero
or more optional sort directives. R is a partitioned permutation of the indices into Y, each
partition sorted according to X.

The →[distinct] function returns the corresponding values for each set of indices.

The group function is often used in conjunction with the →[*.Operators.by] operator.

See also: →[*.distinct], →[*.by], →[*.order]

## Examples

~~~
      V='B,C,C,C,A,C,B,B,A'
      V (distinct V) (group V)
 ┌───────┐  ┌───────┐  ┌─────────┐
 ↓B      │  ↓B      │  ↓[0,6,7]  │
 │C      │  │C      │  │[1,2,3,5]│
 │C      │  │A      │  │[4,8]    │
 │C      │  └Char(1)┘  └Int8─────┘
 │A      │
 │C      │
 │B      │
 │B      │
 │A      │
 └Char(1)┘
      K=7 5 4 9 9 11 6 8 2
      K 'Asc' group V
┌─────────┐
↓[6,0,7]  │
│[2,1,3,5]│
│[8,4]    │
└Int8─────┘
~~~

