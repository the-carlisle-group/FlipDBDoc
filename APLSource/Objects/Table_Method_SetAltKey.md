# SetAltKey Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method creates an alternate key. The key may be simple or compound. FlipDB enforces uniqueness
on alternate keys.

The argument is composed of 1 item:

|-|-|
|1|ColumnNames|Char|

The result is composed of 1 item:

|-|-|
|1|Dummy|0|

For example:

~~~
      T=D.GetTable 'SP'
      T.SetAltKey 'PNO,SNO'
~~~

An alternate key composed of a single column is said to be simple. An alternate key composed of
multiple columns is said to be compound.

FlipDB does not support overlapping keys; therefore, a column may only belong to one key.
Specifying an alternate key automatically removes any existing specification that the columns may
already be a part of. For example, assume table T has one compound alternate key specified,
consisting of columns FIRSTNAME and LASTNAME. Then if:

~~~
      T.SetAltKey 'FIRSTNAME' 'MIDDLENAME'
~~~

the alternate key FIRSTNAME, LASTNAME is removed as a side effect of specifying  the alternate key
FIRSTNAME, MIDDLENAME.

