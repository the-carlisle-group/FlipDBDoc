# PropertySpaces and Functions

PropertySpaces are data structures. FlipDB functions operate on
PropertySpaces and return new PropertySpaces, DataColumns, or other objects.

The →[*.names] function extracts the names in a PropertySpace:
~~~
      P=newPropertySpace 'FirstName,LastName,Rate' ('Adam' 'Smith' 4.875)
      names P
┌─────────┐
↓FirstName│
│LastName │
│Rate     │
└Char(9)──┘
~~~
The →[*.values] function extracts the corresponding values:
~~~
  values P
 ┌───────┐  ┌───────┐  ┌──────┐
 │Adam   │  │Smith  │  │4.875 │
 └Char(4)┘  └Char(5)┘  └Dec(3)┘
~~~
Some of the functions that apply to DataColumns
apply to PropertySpaces in a similar way.
The →[*.index] function extracts values from a PropertySpace
correponding to a list of names:
~~~
      P=newPropertySpace 'Small,Medium,Large' ('12oz' '16oz' '22oz')
      P
┌PropertySpace───────────┐
│ Name         Type      │
│ ---------    --------- │
│ Small        Char      │
│ Medium       Char      │
│ Large        Char      │
└────────────────────────┘
     'Large,Medium,Medium,Large,Small' index P
 ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐
 │22oz   │  │16oz   │  │16oz   │  │22oz   │  │12oz   │
 └Char(4)┘  └Char(4)┘  └Char(4)┘  └Char(4)┘  └Char(4)┘

     enlist 'Large,Medium,Medium,Large,Small' index P
┌───────┐
↓22oz   │
│16oz   │
│16oz   │
│22oz   │
│12oz   │
└Char(4)┘
~~~
The →[*.without] function creates a new PropertySpace by removing name/value pairs:
~~~

~~~
The →[*.join] function creates a new PropertySpace with all of the name/value pairs
from two existing PropertySpaces:
~~~

~~~

