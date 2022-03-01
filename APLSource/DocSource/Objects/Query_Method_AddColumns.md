# AddColumns Method

Applies to:{.prefix}

→[##.##.Query]{.info}

The argument is composed of 1 item:

|-|-|
|1|Names|Char|

The result is composed of 1 item:

|-|-|
|1|Select|DataTable|

This method is used to incrementally add columns to the →[##.Properties.Select] property, and is
useful for adding a large number of columns when there is no need to rename them or specify
expressions. For example:

~~~
      Q.AddColumns 'FirstName,LastName,Address'
Q.AddColumns 'City,State,ZipCode'
~~~

Use the AddColumn method when it is necessary to rename or compute columns.

