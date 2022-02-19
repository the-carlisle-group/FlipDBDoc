# Sort Method

Applies to:{.prefix}

→[##.##.DataTable]{.info}

This method sorts the rows of a DataTable.

The argument is composed of 2 items:

|-|-|
|1|Column(s)|String|
|2|Direction(s)|String|

The result is composed of 1 item:

|-|-|
|1|Instance|DataTable|

For example:

~~~
      T=D.GetTable 'P'
      DT=T.ExecuteQuery ''
      DT.Sort 'COLOR,WEIGHT' 'Up,Down'
~~~

See also: →[*.gradeDown], →[*.gradeUp], →[*.index]

