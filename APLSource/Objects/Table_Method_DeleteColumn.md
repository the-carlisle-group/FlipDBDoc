# DeleteColumn Method

Applies to:{.prefix}

→[##.##.Table]{.info}

Applies to:{.prefix}

→[##.##.Table]{.info}

This method deletes a column.

The argument is composed of 1 item:

|-|-|
|1|ColumnName|String|

The result is composed of 1 item:

|-|-|
|1|Dummy|0|

System columns, primary key and alternate key columns may not be deleted.

For example:

~~~
      T=D.GetTable 'P'
      T.DeleteColumn 'WEIGHT'
~~~

