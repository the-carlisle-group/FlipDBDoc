# distinct

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Extract unique values, in a particular order.{.purpose}

~~~
R=[Y] distinct X
~~~

X may be any column. R contains the unique items of X.
Y is an optional numeric column used to weight the corresponding items in X for sorting the result R.
If Y is not provided, the items in R are ordered according to their first occurance in X.
Otherise the items in R are in descending order of the sum of corresponding items in Y.
If Y is 1 , the items in R are ordered by descending frequency in X.


See also: →[unique]

## Examples

~~~
       distinct 1 2 3
┌────┐
↓1   │
│2   │
│3   │
└Int8┘
       distinct  3 3 3 2 2 1
┌────┐
↓3   │
│2   │
│1   │
└Int8┘

      distinct 'FL,CT,NY,NJ,NY,NY,CT,NJ'
┌───────┐
↓FL     │
│CT     │
│NY     │
│NJ     │
└Char(2)┘

      1 distinct 'FL,CT,NY,NJ,NY,NY,CT,NJ'
┌───────┐
↓NY     │
│CT     │
│NJ     │
│FL     │
└Char(2)┘

      50 5 25 75 10 25 15 25 distinct 'FL,CT,NY,NJ,NY,NY,CT,NJ'
┌───────┐
↓NJ     │
│NY     │
│FL     │
│CT     │
└Char(2)┘
~~~

