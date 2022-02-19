# PropertySpaces

A PropertySpace is set of names and corresponding values.
PropertySpaces are used to gather related data together.
The →[*.newPropertySpace] function creates a new PropertySpace:
~~~
      P=newPropertySpace ''
      P
┌PropertySpace───────────┐
│ Name         Type      │
│ ---------    --------- │
└────────────────────────┘
~~~
A new PropertySpace may be initialized with names and values:
~~~
      P=newPropertySpace 'FirstName,LastName,Rate' ('Adam' 'Smith' 4.875)
      P
┌PropertySpace───────────┐
│ Name         Type      │
│ ---------    --------- │
│ FirstName    Char      │
│ LastName     Char      │
│ Rate         Decimal   │
└────────────────────────┘

~~~
A name/value pair may be added to an existing PropertySpace by dotted
assignment:
~~~
      P.Balance=100000
      P
┌PropertySpace───────────┐
│ Name         Type      │
│ ---------    --------- │
│ FirstName    Char      │
│ LastName     Char      │
│ Rate         Decimal   │
│ Balance      Integer   │
└────────────────────────┘
~~~
Assignment will create a name if it does not exist,
and overwrite the name if it does exist.

A value in the property space is accessed by its name:
~~~
      P.FirstName
┌───────┐
│Adam   │
└Char(4)┘
~~~



