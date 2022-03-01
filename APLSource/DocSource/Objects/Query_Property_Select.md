# Select Property

Applies to:{.prefix}

→[##.##.Query]{.info}

Notes

This property specifies the names of the items, typically DataColumns, to be returned in the result of the query.  For example:

~~~
      Q.Select="PNO,PNAME,COLOR"
~~~

For selecting a large number of columns, the →[##.MethodList.AddColumns] method may be used to
incrementally add columns to the Select property:

~~~
      Q.Select="PNO,PNAME"
      Q.AddColumns "COLOR,WEIGHT"
~~~

A result column name may be specified as a colon separated name and expression pair, which enables renaming:

~~~
      Q.Select="SuplierNumber:SNO,SupplierName:SNAME"
~~~

For queries that require anything more than simple column names or trivial expressions, use the
→[##.MethodList.AddColumn] method to add a single column at a time:

~~~
      Q.AddColumn "TotalWeight" "sum QTY times PNO.WEIGHT"
~~~

The AddColumn method takes an optional third Boolean argument which allows you to specify that
the item is hidden and not returned in the result. This allows the definition of temporary
values that may be referenced further along in the query:

~~~
      Q.AddColumn "TotalQTY" "sum QTY" 1
~~~

Regardless of how the Select property is set, when referenced it always returns an "expression set"-
a three-column DataTable containing Name, Expression and Hide columns. The Select
property may also be specified directly as an expression set.

Items specified by the Select property are not limited to DataColumns and may specifically include
DataTables and PropertySpaces. The result of the executing a query will be a DataTable or a PropertySpace
depending on the types of the items specified in the select property.

If all of the non-hidden items in the Select property are conforming DataColumns,
the query result is a DataTable containing the non-hidden DataColumns.
Otherwise the query result is a PropertySpace containing all of the non-hidden items.

However, if there is only one non-hidden item in the Select property, and the item is
itself a DataTable or a PropertySpace, then this item is the query result.
In other words, a lone DataTable or PropertySpace item is not further encapsulated
in a PropertySpace.


