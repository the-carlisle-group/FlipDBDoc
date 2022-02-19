# Execute Method

Applies to:{.prefix}

→[##.##.Query]{.info}

This method evaluates the query.

The argument is composed of 1 item:

|-|-|
|1|AsOf| 0 or 1 or DateTime or negative integer|

The result is composed of 1 item:

|-|-|
|1|Query result|DataTable or PropertySpace|

If all of the non-hidden items in the Select property are conforming DataColumns,
the result is a DataTable containing the non-hidden DataColumns.
Otherwise the results is a PropertySpace containing all of the non-hidden items.

However, if there is only one non-hidden item in the Select property, and the item is
itself a DataTable or a PropertySpace, then this item is the result.
In other words, a lone DataTable or PropertySpace item is not further encapsulated
in a PropertySpace.

AsOf parameter defaults to 0, which indicates the current or latest state of the database. If a
DateTime is specified the query is applied to the database as it appeared at that time. If a
negative integer N is provided, the database is rolled back M transactions, where M is the
absolute value of N (for purposes of the query only).

## Examples

To select suppliers in the city of London:

~~~
      T=D.GetTable 'S'
      Q=T.Query 'CITY in "London"' 'SNO,SNAME'
      DT=Q.Execute 0
~~~

To see the suppliers and parts table as it appeared before the last transaction:

~~~
      T=D.GetTable 'SP'
      Q=T.Query ''
      DT=Q.Execute -1
~~~

To see the parts table as it appeared at year end 2005:

~~~
      T=D.GetTable 'P'
      Q=T.Query ''
      R=Q.Execute 20051231.235959
~~~

