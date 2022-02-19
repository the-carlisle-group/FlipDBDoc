# DeferredInsert Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method returns a new Assignment object of type "Insert".

The argument is composed of 1 item:

|-|-|
|1|Data|DataTable|

The result is composed of one item:

|-|-|
|1|Assignment|Assignment object|

The result of this method is an →[*.Objects.Assignment] object that is usually composed with other
Assignment objects and passed to the →[*.Database.MethodList.Assign] method which  then executes
a multiple assignment.

For example, to add 1 row to each table in the suppliers and parts database, ensuring that the
entire transaction is atomic:

~~~
      ST=D.GetTable 'S'
      PT=D.GetTable 'P'
      SP=D.GetTable 'SP'
      DT1=('SNO' 'S6') ('SNAME' 'Codd') ('CITY' 'London')
      A1=ST.DeferredInsert DT1
      DT2=('PNO' 'P7' )('PNAME' 'Chip') ('WEIGHT' 15)
      A2=PT.DeferredInsert DT2
      DT3=('SNO' 'S6' )('PNO' 'P7') ('QTY' 350)
      A3=SP.DeferredInsert DT3
      D.Assign A1 A2 A3
~~~

See also: →[DeferredUpdate], →[DeferredDelete]

