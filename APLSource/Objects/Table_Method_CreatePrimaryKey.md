# CreatePrimaryKey Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method creates a primary key column.

The argument is composed of 2 items:

|-|-|
|1|Column name|String|
| 2|Column type|String|

The result is composed of 1 item:

|-|-|
|1|Primary key|Column object|

Creating a primary key will clear any previously defined primary key column.

The instance table must be “new”, having no rows previously added or deleted.

Primary keys may not be compound and may not be of type Blob.

For example:

~~~
      T=D.CreateTable 'PhoneNumbers'
      PK=T.CreatePrimaryKey 'PhoneNumber' 'Char(15)
~~~

