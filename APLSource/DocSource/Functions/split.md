# split

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Text{.info}

Split a string into multiple strings.{.purpose}

~~~
R=[X] split Y
~~~

Y is a scalar or simple char column. X may be character or integer.
X defaults to single blank.

If X is character R is an enclosed
or partitioned char column, with the items in Y split into multiple items based
on the delimiters X. Leading and trailing delimiters are ignored,
and consecutive delimiters are treated as one.

If X is an integer, the items in Y are split into groups of characters
of length X.

This function is often used with the →[first], →[last] and →[nth] functions to extract words from a string.

See also →[splitCSV].

## Examples:

~~~
      a='Edgar Allan Poe,Adam Smith,Socrates,Chris J. Date'
      a
┌───────────────┐
↓Edgar Allan Poe│
│Adam Smith     │
│Socrates       │
│Chris J. Date  │
└Char(15)───────┘
      split a
┌─────────────────┐
↓[Edgar,Allan,Poe]│
│[Adam,Smith]     │
│[Socrates]       │
│[Chris,J.,Date]  │
└Char(8)──────────┘
      last split a
┌────────┐
↓Poe     │
│Smith   │
│Socrates│
│Date    │
└Char(8)─┘

      A='abc,defg,hijkl'
      A (1 split A) (2 split A) (3 split A)
 ┌───────┐  ┌───────────┐  ┌─────────┐  ┌────────┐
 ↓abc    │  ↓[a,b,c]    │  ↓[ab,c]   │  ↓[abc]   │
 │defg   │  │[d,e,f,g]  │  │[de,fg]  │  │[def,g] │
 │hijkl  │  │[h,i,j,k,l]│  │[hi,jk,l]│  │[hij,kl]│
 └Char(5)┘  └Char(1)────┘  └Char(2)──┘  └Char(3)─┘

~~~

