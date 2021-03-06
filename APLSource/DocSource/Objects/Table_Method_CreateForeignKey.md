# CreateForeignKey Method

Applies to:{.prefix}

β[##.##.Table]{.info}

This method creates a foreign key column.

The argument is composed of 2 items:

|-|-|
|1| Column name| String|
| 2| Foreign table name| String|

The result is a β[*.Objects.Column] object, containing the newly created foreign key.

Foreign keys must refer to the primary key of the foreign table. Therefore, the type of the foreign
key is implied by the primary key of the foreign table.

The instance table must be βnewβ, having no rows previously added or deleted.

Foreign keys may not be compound.

For example:

~~~
      N=D.CreateTable 'PhoneNumbers'
      FK=N.CreateForeignKey 'SNO' 'S'
~~~

