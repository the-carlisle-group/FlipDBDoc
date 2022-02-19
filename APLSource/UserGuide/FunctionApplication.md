# Function Application

We have already seen how to apply a single search pattern to each row
in a DataColumn. This is the most common application.

However, regex functions may be applied in a number of ways. The structure of the
arguments determines how the function is applied.

The regex family of functions accept multiple search patterns, all
of which are applied to each row in the subject column:

~~~
      C='NY and NJ,Only in CT,NY, NJ, and CT'
      C (regexMatch C ('NY' 'NJ' 'CT'))
 ┌──────────────┐  ┌──────────┐
 ↓NY and NJ     │  ↓[NY,NJ]   │
 │Only in CT    │  │[CT]      │
 │NY, NJ, and CT│  │[NY,NJ,CT]│
 └Char(14)──────┘  └Char(2)───┘

~~~

Here we have applied the same set of search patterns to each row
of the DataColumn. Note that is this is equivalent to using
a single search pattern with alternation:

~~~
      C (regexMatch C 'NY|NJ|CT')
 ┌──────────────┐  ┌──────────┐
 ↓NY and NJ     │  ↓[NY,NJ]   │
 │Only in CT    │  │[CT]      │
 │NY, NJ, and CT│  │[NY,NJ,CT]│
 └Char(14)──────┘  └Char(2)───┘

~~~

We can then replace each match with a single replace pattern:

~~~
      C (regexReplace C ('NY' 'NJ' 'CT') 'SomeState')
 ┌──────────────┐  ┌───────────────────────────────────┐
 ↓NY and NJ     │  ↓SomeState and SomeState            │
 │Only in CT    │  │Only in SomeState                  │
 │NY, NJ, and CT│  │SomeState, SomeState, and SomeState│
 └Char(14)──────┘  └Char(35)───────────────────────────┘

~~~

Or we can replace each match with a corresponding replace pattern:

~~~
      C (regexReplace C ('NY' 'NJ' 'CT') ('New York' 'New Jersey' 'Connecticut'))
 ┌──────────────┐  ┌─────────────────────────────────────┐
 ↓NY and NJ     │  ↓New York and New Jersey              │
 │Only in CT    │  │Only in Connecticut                  │
 │NY, NJ, and CT│  │New York, New Jersey, and Connecticut│
 └Char(14)──────┘  └Char(37)─────────────────────────────┘

~~~

Rather than applying the same set of one or more search patterns to each
row of the subject column as above, we can apply a different search pattern to
each row. In other words, the seach patterns themselves vary by row.

Consider the following 3 columns as part of a table:

~~~
      Salutation='Dear Dr. and Mrs. Codd,Dear Mrs. Rodgers and Family,Greetings Lord Vader'
      LastName='Codd,Rodgers,Vader'
      FullName='E.F. Codd,William James Rodgers,Darth Vader'
      Salutation LastName FullName
 ┌────────────────────────────┐  ┌───────┐  ┌─────────────────────┐
 ↓Dear Dr. and Mrs. Codd      │  ↓Codd   │  ↓E.F. Codd            │
 │Dear Mrs. Rodgers and Family│  │Rodgers│  │William James Rodgers│
 │Greetings Lord Vader        │  │Vader  │  │Darth Vader          │
 └Char(28)────────────────────┘  └Char(7)┘  └Char(21)─────────────┘
~~~

We can search the Salutation for the LastName and replace it with the FullName:

~~~
      regexReplace Salutation LastName FullName
┌──────────────────────────────────────────┐
↓Dear Dr. and Mrs. E.F. Codd               │
│Dear Mrs. William James Rodgers and Family│
│Greetings Lord Darth Vader                │
└Char(42)──────────────────────────────────┘
~~~

All of this is controlled by the structure of the search pattern.
If the search pattern is simple, it must correspond in length to
the subject column. In this case, the search patterns vary by row,
and corresponding rows are searched for the corresponding pattern.
In all other cases, all of the search patterns are applied to each row,
and the result is equivalent to a single search pattern with alternation.
Note that this equivalence does not negate the usefullness of multiple
seach patterns, especially with respect to replace operations.

