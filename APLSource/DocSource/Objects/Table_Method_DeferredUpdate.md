# DeferredUpdate Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method returns a new Assignment object of type "Update".

The argument is composed of 1 item:

|-|-|
|1|Data|DataTable|

The result is composed of one item:

|-|-|
|1|Assignment|Assignment object|

The result of this method is an →[*.Objects.Assignment] object that is usually composed with other
Assignment objects and passed to the →[*.Database.MethodList.Assign] method which then executes a
multiple assignment.

For example, to update 1 row in each of two tables in the suppliers and parts database, ensuring
that the entire transaction is atomic:

~~~
      ST=D.GetTable 'S'
      PT=D.GetTable 'P'
      DT1=('SNO' 'S6') ('STATUS' 70)
      A1=ST.DeferredAdd DT1
      DT2=('PNO' 'P2' )('WEIGHT' 37)
      A2=PT.DeferredAdd DT2
      D.Assign A1 A2
~~~

See also: →[DeferredInsert], →[DeferredDelete]

