# CreateColumn Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method creates a new column.

The argument is composed of 2 items:

|-|-|
|1|Column name|String|
| 2|Column type|String|

The result is a Column object.

For example:

~~~
      T=D.GetTable 'S'
      C=T.CreateColumn 'ADDRESS' 'Char(32)'
~~~

Column names must begin with an uppercase letter and may contain upper and lower case letters, the
digits 0-9, and underscores.

Column types include:

* Char(w),CharW(w),CharXW(w),Boolean,Int8,Int16,Int32,Dec(d),Float,Date,Time,DateTime,Blob

