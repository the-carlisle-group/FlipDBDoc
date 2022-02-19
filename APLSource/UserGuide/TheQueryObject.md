# The Query Object

To examine the query object in detail, let's extract the suppliers and parts cross-reference table:

~~~
      SP=D.GetTable 'SP'
~~~

and then obtain a Query object:

~~~
      Q=SP.Query ''
~~~

Using the default properties, the query may be executed, returning all of the rows and columns:

~~~
      Q.Execute 0
────────────────────────────────────────────────────────
 ┌AUTOKEY┐  ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌SDATE─────┐
 ↓ 1     │  ↓S1     │  ↓P1     │  ↓300  │  ↓2008-10-27│
 │ 2     │  │S1     │  │P2     │  │200  │  │2008-08-19│
 │ 3     │  │S1     │  │P3     │  │400  │  │2008-05-18│
 │ 4     │  │S1     │  │P4     │  │200  │  │2007-08-18│
 │ 5     │  │S1     │  │P5     │  │100  │  │2007-08-17│
 │ 6     │  │S1     │  │P6     │  │100  │  │2009-12-16│
 │ 7     │  │S2     │  │P1     │  │300  │  │2009-07-31│
 │ 8     │  │S2     │  │P2     │  │400  │  │2009-12-17│
 │ 9     │  │S3     │  │P2     │  │200  │  │2009-07-24│
 │10     │  │S4     │  │P2     │  │200  │  │2009-10-03│
 │11     │  │S4     │  │P4     │  │300  │  │2009-06-28│
 │12     │  │S4     │  │P5     │  │400  │  │2009-11-04│
 └Int8───┘  └Char(2)┘  └Char(2)┘  └Int16┘  └Date──────┘
── 12 rows by 5 columns ────────────────────────────────
~~~

Now let's examine some of the basic properties of the query object. The →[*.Where] property
specifies a Boolean expression to restrict the number of rows:

~~~
      Q.Where='QTY > 200'
      Q.Execute 0
────────────────────────────────────────────────────────
 ┌AUTOKEY┐  ┌QTY──┐  ┌SDATE─────┐  ┌SNO────┐  ┌PNO────┐
 ↓ 1     │  ↓300  │  ↓2008-10-27│  ↓S1     │  ↓P1     │
 │ 3     │  │400  │  │2008-05-18│  │S1     │  │P3     │
 │ 7     │  │300  │  │2009-07-31│  │S2     │  │P1     │
 │ 8     │  │400  │  │2009-12-17│  │S2     │  │P2     │
 │11     │  │300  │  │2009-06-28│  │S4     │  │P4     │
 │12     │  │400  │  │2009-11-04│  │S4     │  │P5     │
 └Int8───┘  └Int16┘  └Date──────┘  └Char(2)┘  └Char(2)┘
── 6 rows by 5 columns ─────────────────────────────────
~~~

If we inspect the Where property, we see that it is actually a DataTable that may contain zero or
more rows, rather than a simple string:

~~~
      Q.Where
───────────────────────
 ┌Expression┐
 ↓QTY > 200 │
 └Char(9)───┘
── 1 rows by 1 columns
~~~

Multiple where statements are evaluated independently and then logically "and-ed" together. While
the Where property may be directly specified as a DataTable, the →[*.AddWhere] method provides an
easy way to incrementally add a where statement:

~~~
      Q.AddWhere 'PNO in "P1,P2,P3,P4"'
────────────────────────
 ┌Expression──────────┐
 ↓QTY > 200           │
 │PNO in 'P1,P2,P3,P4'│
 └Char(20)────────────┘
── 2 rows by 1 columns ─
~~~

If we execute the query again, we exclude the row containing part number P5:

~~~
      Q.Execute 0
── Key:AUTOKEY ─────────────────────────────────────────
 ┌AUTOKEY┐  ┌QTY──┐  ┌SDATE─────┐  ┌SNO────┐  ┌PNO────┐
 ↓1      │  ↓300  │  ↓2008-10-27│  ↓S1     │  ↓P1     │
 │3      │  │400  │  │2008-05-18│  │S1     │  │P3     │
 │7      │  │300  │  │2009-07-31│  │S2     │  │P1     │
 │8      │  │400  │  │2009-12-17│  │S2     │  │P2     │
 │11     │  │300  │  │2009-06-28│  │S4     │  │P4     │
 └Int8───┘  └Int16┘  └Date──────┘  └Char(2)┘  └Char(2)┘
── 5 rows by 5 columns ─────────────────────────────────
~~~

As we have noted, the statements in the where clause are logically "and-ed" together. Thus the
above two-item where clause is equivalent to the single where statement:

~~~
      '(QTY > 200) and (PNO in "P1,P2,P3,P4")'
┌──────────────────────────────────────┐
│(QTY > 200) and (PNO in "P1,P2,P3,P4")│
└Char(38)──────────────────────────────┘
~~~

There are at least three very good reasons for breaking a complex where clause into multiple
independent statements.  First, and most important, is human readability. It is much easier to
write, read, and maintain multiple short statements rather than one long, parenthesis ridden
statement. Second is business logic. Often a long where clause encapsulates multiple business
rules or concepts. If each rule is stated separately, then each rule may be individually named and
analyzed. In addition, the interaction between each and every rule may be analyzed. This topic is
covered in detail in the section on "Statement Analysis." Third is computational speed and
efficiency. As FlipDB processes each Boolean statement in a where clause, it can reduce the amount
of data it needs to look at. This implies that to the extent possible, where statements should be
ordered so that the most restrictive statement is first, and the least restrictive is last. In
other words, remove as much data from the problem as soon as possible. Note that this bit of user
optimization should only be worried about when large data sets are involved.  There is no need to
order your where statements when working with, say, under a million rows.

The →[*.WhereQuota] specifies a maximum number of rows that the Where property may return. Its
default value is 0, which indicates all rows. This is useful for restricting the number of rows
returned from large tables:

~~~
      Q.WhereQuota=3
      Q.Execute 0
── Key:AUTOKEY ─────────────────────────────────────────
 ┌AUTOKEY┐  ┌QTY──┐  ┌SDATE─────┐  ┌SNO────┐  ┌PNO────┐
 ↓1      │  ↓300  │  ↓2008-10-27│  ↓S1     │  ↓P1     │
 │3      │  │400  │  │2008-05-18│  │S1     │  │P3     │
 │7      │  │300  │  │2009-07-31│  │S2     │  │P1     │
 └Int8───┘  └Int16┘  └Date──────┘  └Char(2)┘  └Char(2)┘
── 3 rows by 5 columns ─────────────────────────────────
~~~

Note that the 3 rows selected by the WhereQuota are off the "top" of the table. The rows of FlipDB
tables have a natural ordering that is defined by the time that a row was added or last updated.
Recently added or updated rows will be at the bottom of the table, while older rows will be at the
top. The WhereQuota property will apply to the bottom of table if it assigned a negative integer:

~~~
      Q.WhereQuota= -3
      Q.Execute 0
── Key:AUTOKEY ─────────────────────────────────────────
 ┌AUTOKEY┐  ┌QTY──┐  ┌SDATE─────┐  ┌SNO────┐  ┌PNO────┐
 ↓7      │  ↓300  │  ↓2009-07-31│  ↓S2     │  ↓P1     │
 │8      │  │400  │  │2009-12-17│  │S2     │  │P2     │
 │11     │  │300  │  │2009-06-28│  │S4     │  │P4     │
 └Int8───┘  └Int16┘  └Date──────┘  └Char(2)┘  └Char(2)┘
── 3 rows by 5 columns ─────────────────────────────────
~~~

As was shown above, the Select property may be specified as an argument to the Query method.
However, select clauses are often lengthy and complex, and specifying one in a single statement
can make it hard to read. The Query object provides a number of techniques for specifying it in a
clear and readable format. The →[*.Select] property may be directly specified as a list of column names:

~~~
      Q.Select='SNO,PNO'
~~~

Note that while the Select property is specified here as a list of names, its formal definition is
a 3-column DataTable, which allows for renaming and specifying whether or not the column is
returned in the result:

~~~
      Q.Select
────────────────────────────────────
 ┌Name───┐  ┌Expression┐  ┌Visible┐
 ↓SNO    │  ↓SNO       │  ↓1      │
 │SDATE  │  │SDATE     │  │1      │
 └Char(5)┘  └Char(5)───┘  └Boolean┘
── 2 rows by 3 columns ─────────────
~~~

It may be wondered why it would be useful to specify a column in the result, only to additionally
specify that it should not be visible. Observe that the title of the 2nd column of the Select
DataTable is "Expression" and not "Column". The Select DataTable is in fact a list of named
expressions, executed by the query in order. Each expression can reference names defined above it,
as well as all column names in the database. It is often useful to define temporary values that
may be used repeatedly in following expressions, both for performance, and for clarity, and yet
not have these temporary columns clutter up the result. This will be shown below.

Columns may be incrementally added to the Select property by using the
→[*.Query.MethodList.AddColumns] method:

~~~
      J=Q.AddColumns 'PNO,QTY'
      Q.Select
────────────────────────────────────
 ┌Name───┐  ┌Expression┐  ┌Visible┐
 ↓SNO    │  ↓SNO       │  ↓1      │
 │SDATE  │  │SDATE     │  │1      │
 │PNO    │  │PNO       │  │1      │
 │QTY    │  │QTY       │  │1      │
 └Char(5)┘  └Char(5)───┘  └Boolean┘
── 4 rows by 3 columns ─────────────
~~~

Repeated use of the AddColumns method is useful for constructing queries with large numbers of
columns in the result. It is also possible to rename columns, both when specifying the Select
property directly and when using the AddColumns method:

~~~
      Q.Select='SupplierNumber:SNO,PNO'
      Q.AddColumns 'SDATE,Quantity:QTY'
───────────────────────────────────────────
 ┌Name──────────┐  ┌Expression┐  ┌Visible┐
 ↓SupplierNumber│  ↓SNO       │  ↓1      │
 │PNO           │  │PNO       │  │1      │
 │SDATE         │  │SDATE     │  │1      │
 │Quantity      │  │QTY       │  │1      │
 └Char(14)──────┘  └Char(5)───┘  └Boolean┘
── 4 rows by 3 columns ────────────────────
~~~

The →[*.Query.MethodList.AddColumn] method incrementally adds one column to the Select property.
You must explicitly specify a name, expression, and optionally a visible flag:

~~~
      J=Q.AddColumn 'Weight' 'PNO.WEIGHT'
~~~

In this case, the foreign key PNO is used to "hop" to the parts table and extract the WEIGHT
column. (This syntax is explained in detail below in the section on joins) Column definitions may
be expressions, as well as simple names, and expressions may refer to names previously defined in
the Select property:

~~~
      J=Q.AddColumn 'ExtendedWeight' 'Weight * QTY'
~~~

Let's review the Select property:

~~~
      Q.Select
──────────────────────────────────────────────────
 ┌Name──────────┐  ┌Expression───────┐  ┌Visible┐
 ↓SupplierNumber│  ↓SNO              │  ↓1      │
 │PNO           │  │PNO              │  │1      │
 │SDATE         │  │SDATE            │  │1      │
 │Quantity      │  │QTY              │  │1      │
 │Weight        │  │PNO.WEIGHT       │  │1      │
 │ExtendedWeight│  │Weight * Quantity│  │1      │
 └Char(14)──────┘  └Char(17)─────────┘  └Boolean┘
── 6 rows by 3 columns ───────────────────────────
~~~

And run the query:

~~~
      Q.Execute 0
───────────────────────────────────────────────────────────────────────────────────
 ┌SupplierNumber┐  ┌PNO────┐  ┌SDATE─────┐  ┌Quantity┐  ┌Weight┐  ┌ExtendedWeight┐
 ↓S2            │  ↓P1     │  ↓2009-07-31│  ↓300     │  ↓12    │  ↓3,600         │
 │S2            │  │P2     │  │2009-12-17│  │400     │  │17    │  │6,800         │
 │S4            │  │P4     │  │2009-06-28│  │300     │  │14    │  │4,200         │
 └Char(2)───────┘  └Char(2)┘  └Date──────┘  └Int16───┘  └Int8──┘  └Int16─────────┘
── 3 rows by 6 columns ────────────────────────────────────────────────────────────
~~~

Remember that in addition to the Select property, we have specified the Where and WhereQuota
properties as well to achieve this result.

The WhereNot property is analogous to the Where property, but rather than identifying rows to keep,
it flags row to remove. Furthermore, multiple WhereNot expressions are logically "or-ed" together
rather than "and-ed" together. Let's remove any shipment whose shipment is after December 16th:

~~~
      Q.WhereNot='SDATE > "2009-12-16"'
~~~

As well as any shipment of parts whose unit weight is less than 14:

~~~
      Q.AddWhereNot 'PNO.WEIGHT < 14'
────────────────────────
 ┌Expression──────────┐
 ↓SDATE > '2009-12-16'│
 │PNO.WEIGHT < 14     │
 └Char(20)────────────┘
── 2 rows by 1 columns ─
~~~

Because multiple WhereNot statements are "or-ed" together, a row will be removed if either of these
two conditions is true. Let's execute the query and see the result:

~~~
      Q.Execute 0
───────────────────────────────────────────────────────────────────────────────────
 ┌SupplierNumber┐  ┌PNO────┐  ┌SDATE─────┐  ┌Quantity┐  ┌Weight┐  ┌ExtendedWeight┐
 ↓S1            │  ↓P3     │  ↓2008-05-18│  ↓400     │  ↓17    │  ↓6,800         │
 │S4            │  │P4     │  │2009-06-28│  │300     │  │14    │  │4,200         │
 └Char(2)───────┘  └Char(2)┘  └Date──────┘  └Int16───┘  └Int8──┘  └Int16─────────┘
── 2 rows by 6 columns ────────────────────────────────────────────────────────────
      Q.WhereNot
~~~

Note that while two rows have been removed from the previous result, a new row has popped in. This
perhaps unexpected result is because we still have a WhereQuota of -3 set, which was binding in
the previous result, but now, with our additional restrictions in the WhereNot clause, is no
longer binding. Let's review the entire Query object's properties:

~~~
      Q
── Query ────────────────────────────────────────────
DatabaseName: SandP
TableName: SP
AsOf: 2
Where:  ┌Expression──────────┐
        ↓QTY > 200           │
        │PNO in 'P1,P2,P3,P4'│
        └Char(20)────────────┘
WhereNot:  ┌Expression──────────┐
           ↓SDATE > '2009-12-16'│
           │PNO.WEIGHT < 14     │
           └Char(20)────────────┘
WhereQuota: -3
GroupBy:
AllowOverlappingGroups: 0
IndependentGroups: 0
KeepEmptyGroups: 1
IncludeTotalRow: 0
Select:  ┌Name──────────┐  ┌Expression──┐  ┌Visible┐
         ↓SupplierNumber│  ↓SNO         │  ↓1      │
         │PNO           │  │PNO         │  │1      │
         │SDATE         │  │SDATE       │  │1      │
         │Quantity      │  │QTY         │  │1      │
         │Weight        │  │PNO.WEIGHT  │  │1      │
         │ExtendedWeight│  │Weight * QTY│  │1      │
         └Char(14)──────┘  └Char(12)────┘  └Boolean┘
OrderBy:
Having:
HavingQuota: 0
RollUp: 0
─────────────────────────────────────────────────────
~~~

