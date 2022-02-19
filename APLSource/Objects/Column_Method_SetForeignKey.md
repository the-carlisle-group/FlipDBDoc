# SetForeignKey Method

Applies to:{.prefix}

→[##.##.Column]{.info}

This method makes the column a foreign key, referencing the primary key column of another table.

The argument is composed of 1 item:

|-|-|
|1|Table name|String or Table object|

|-|-|
|1|Instance|Column|

For example, to make column C a foreign key referencing the primary key column of table T:

~~~
      C.SetForeignKey T
~~~

There is no choice as to the column to reference; it must be the primary key of the referenced
table, and therefore only the Table object is required as input to the method.

The column and the primary key of the referenced table must be of compatible types.

Use the →[*.Dereference] method to remove a foreign key specification.

