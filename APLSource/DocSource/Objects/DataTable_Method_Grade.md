# Grade Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

This method generates a permutation of the row indices of a table suitable for sorting.

The argument is composed of 2 items:

|-|-|
|1|Column(s)|String|
|2|Direction(s)|String|

The result is composed of 1 item:

|-|-|
|1|Indices|Integer|

For example:

~~~
      T=D.GetTable 'P'
      DT=T.ExecuteQuery ''
      G=DT.Grade 'COLOR,WEIGHT' 'Up,Down'
      DT.Index G
~~~

Note that the above is equivalent to:

~~~
      DT.Sort 'COLOR,WEIGHT' 'Up,Down'
~~~

While this is more succinct, it is often useful to have the indices G available to sort other
corresponding objects.

