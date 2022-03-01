# Grouped Queries

Grouping reduces the number of rows returned by combining rows that have like values in a
particular column, or across a set of columns. To explore grouping, let's first create a fresh
Query object from the suppliers and parts table:

~~~
      Q=T.Query ''
~~~

And then add a couple of columns to the Select property:

~~~
      Q.Select='QTY,PNO'
~~~

The →[*.GroupBy] property specifies grouping by one or more columns or expressions. When grouping
by a single column, the column may be specified directly:

~~~
      Q.GroupBy='Supplier' 'SNO'
      Q.Execute 0
────────────────────────────────────────────────────────────────
 ┌Supplier┐  ┌QTY──────────────────────┐  ┌PNO────────────────┐
 ↓S1      │  ↓[300,200,400,200,100,100]│  ↓[P1,P2,P3,P4,P5,P6]│
 │S2      │  │[300,400]                │  │[P1,P2]            │
 │S3      │  │[200]                    │  │[P2]               │
 │S4      │  │[200,300,400]            │  │[P2,P4,P5]         │
 │S5      │  │[]                       │  │[]                 │
 └Char(2)─┘  └Int16────────────────────┘  └Char(2)────────────┘
── 5 rows by 3 columns ─────────────────────────────────────────
~~~

Note that grouping has occurred, but the result columns are not aggregated. In FlipDB there is no
requirement to aggregate for grouped queries; Grouping and aggregation are completely independent.
If aggregation is not done, the columns resulting from the Select property are partitioned, rather
than simple. However, in most cases, aggregation is desired, and is  accomplished by applying one
or more aggregate functions. Let's clear out the existing Select property:

~~~
      Q.Select=''
~~~

Now, we will use the AddColumn method to incrementally add columns to the Select property. First,
we will use the aggregate function count to count up the number of part numbers supplied by each supplier:

~~~
      J=Q.AddColumn 'UniqueParts' 'count PNO'
~~~

Now let's use the aggregate function sum to add the total quantity:

~~~
      J=Q.AddColumn 'TotalQuantity' 'sum QTY'
~~~

And finally the total weight:

~~~
      J=Q.AddColumn 'TotalWeight' 'sum QTY * PNO.WEIGHT'
~~~

Let's execute the query and inspect the result:

~~~
      Q.Execute 0
───────────────────────────────────────────────────────────
 ┌Supplier┐  ┌UniqueParts┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓S1      │  ↓6          │  ↓1300         │  ↓19700      │
 │S2      │  │2          │  │ 700         │  │10400      │
 │S3      │  │1          │  │ 200         │  │ 3400      │
 │S4      │  │3          │  │ 900         │  │12400      │
 │S5      │  │0          │  │   0         │  │    0      │
 └Char(2)─┘  └Int8───────┘  └Int16────────┘  └Int16──────┘
── 5 rows by 4 columns ────────────────────────────────────
~~~

Using the →[*.Query.MethodList.AddColumn] method to build up the →[*.Select] property one column
at a time is ideal for grouped queries, which usually require non-trivial expressions and explicit
column naming.

Note that in this result supplier S5 has no parts associated with it, and in fact nowhere appears
in the suppliers and parts table. However, because SNO is a foreign key, the default behavior of
the query object is to provide a group for each of its values as they appear in the supplier
table. Empty groups may be removed by setting the →[*.KeepEmptyGroups] property to 0:

~~~
      Q.KeepEmptyGroups=0
      Q.Execute 0
───────────────────────────────────────────────────────────
 ┌Supplier┐  ┌UniqueParts┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓S1      │  ↓6          │  ↓1,300        │  ↓19,700     │
 │S2      │  │2          │  │700          │  │10,400     │
 │S3      │  │1          │  │200          │  │3,400      │
 │S4      │  │3          │  │900          │  │12,400     │
 └Char(2)─┘  └Int8───────┘  └Int16────────┘  └Int16──────┘
── 4 rows by 4 columns ────────────────────────────────────
~~~

When grouping by a numeric or temporal column, it is often useful to group by a range of values,
rather than the unique values of the column, which may be excessive in number. This is easily
accomplished by using the →[*.range] function to transform the column we are grouping on.
Consider grouping by the QTY column, in buckets of 250:

~~~
      Q.GroupBy='Quantity' '1 1000 250 range QTY'
      Q.Execute 0
─────────────────────────────────────────────────────────────
 ┌Quantity──┐  ┌UniqueParts┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓1 to 250  │  ↓6          │  ↓1,000        │  ↓16,100     │
 │251 to 500│  │6          │  │2,100        │  │29,800     │
 └Int16─────┘  └Int8───────┘  └Int16────────┘  └Int16──────┘
── 2 rows by 4 columns ──────────────────────────────────────
~~~

The left argument of the range function specifies the desired starting point, ending point, and
bucked size. Because it is possible that no data falls into a specified bucket, grouping by ranged
data can create empty groups just like grouping by a foreign key column. We can turn the
KeepEmptyGroups property back on:

~~~
      Q.KeepEmptyGroups=1
~~~

And re-execute the query to see the empty groups generated by our range specification:

~~~
      Q.Execute 0
───────────────────────────────────────────────────────────────
 ┌Quantity────┐  ┌UniqueParts┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓1 to 250    │  ↓6          │  ↓1,000        │  ↓16,100     │
 │251 to 500  │  │6          │  │2,100        │  │29,800     │
 │501 to 750  │  │0          │  │0            │  │0          │
 │751 to 1,000│  │0          │  │0            │  │0          │
 └Int16───────┘  └Int8───────┘  └Int16────────┘  └Int16──────┘
── 4 rows by 4 columns ────────────────────────────────────────
~~~

In addition to grouping by the unique values of a column or set of columns, and grouping by ranged
columns, FlipDB can group by an arbitrary partition defined by a set of Boolean valued statements.
Consider pooling the shipments into four groups, the first based on parts in the city of Oslo:

~~~
      Q.GroupBy='Group1' 'PNO.CITY in "Oslo"'
~~~

The second including ship dates in 2009 and after:

~~~
      J=Q.AddGroupBy 'Group2' 'SDATE > "2009-01-01"'
~~~

The third with ship dates earlier than 2009:

~~~
      J=Q.AddGroupBy 'Group3' 'SDATE <= "2009-01-01"'
~~~

And the final group based on parts located in Rome:

~~~
      Q.AddGroupBy 'Group4' 'PNO.CITY in "Rome"'
────────────────────────────────────
 ┌Name───┐  ┌Expression───────────┐
 ↓Group1 │  ↓PNO.CITY in 'Oslo'   │
 │Group2 │  │SDATE > '2009-01-01' │
 │Group3 │  │SDATE <= '2009-01-01'│
 │Group4 │  │PNO.CITY in 'Rome'   │
 └Char(6)┘  └Char(21)─────────────┘
── 4 rows by 2 columns ─────────────
~~~

Now let's execute the query and see the result:

~~~
      Q.Execute 0
────────────────────────────────────────────────────────────
 ┌Partition┐  ┌UniqueParts┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓Group1   │  ↓1          │  ↓400          │  ↓6,800      │
 │Group2   │  │7          │  │1,900        │  │28,100     │
 │Group3   │  │4          │  │800          │  │11,000     │
 │Group4   │  │0          │  │0            │  │0          │
 └Char(6)──┘  └Int8───────┘  └Int16────────┘  └Int16──────┘
── 4 rows by 4 columns ─────────────────────────────────────
~~~

Note that each group is defined by a statement, or boolean valued expression. This means it is
possible that some groups may have no rows in them, just as in grouping by a foreign key or a
ranged column. In addition, it means that there may in fact be overlapping groups. The default
behavior of a query is to disallow overlapping groups by successively removing rows from
consideration as each group is processed. Thus, Group3 contains all rows with supplier dates
before 2009 that have not already been selected by Group1 or Group2. We can change this behavior
by turning on the AllowOverlappingGroups property:

~~~
      Q.AllowOverlappingGroups=1
~~~

And then re-running the query:

~~~
      Q.Execute 0
────────────────────────────────────────────────────────────
 ┌Partition┐  ┌UniqueParts┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓Group1   │  ↓1          │  ↓400          │  ↓6,800      │
 │Group2   │  │7          │  │1,900        │  │28,100     │
 │Group3   │  │5          │  │1,200        │  │17,800     │
 │Group4   │  │0          │  │0            │  │0          │
 └Char(6)──┘  └Int8───────┘  └Int16────────┘  └Int16──────┘
── 4 rows by 4 columns ─────────────────────────────────────
~~~

Here we see that Group3 has picked up another row from Group1, indicating that in fact there are
overlapping groups.

Regardless of how groups are defined, the IncludeTotalRow property may be turned on to add a total
row to the bottom of the result of a grouped query:

~~~
      Q.IncludeTotalRow=1
      Q.Execute 0
────────────────────────────────────────────────────────────
 ┌Partition┐  ┌UniqueParts┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓Group1   │  ↓1          │  ↓400          │  ↓6,800      │
 │Group2   │  │7          │  │1,900        │  │28,100     │
 │Group3   │  │5          │  │1,200        │  │17,800     │
 │Group4   │  │0          │  │0            │  │0          │
 │         │  │12         │  │3,100        │  │45,900     │
 └Char(6)──┘  └Int8───────┘  └Int16────────┘  └Int32──────┘
── 5 rows by 4 columns ─────────────────────────────────────
~~~

Note that the total line does not double count rows when there are overlapping groups, as there are
in this case.

It is often useful to inspect the result of a query after the GroupBy and Select properties have
been evaluated, and then apply further analysis to the returned rows. This can be accomplished
with the →[*.OrderBy], →[*.Having] and →[*.HavingQuota] properties. These three properties
operate on the columns defined by the Select property. Let's create and execute a fresh query on
the shipments table, grouped by supplier:

~~~
      Q=T.Query ''
      Q.GroupBy='Supplier' 'SNO'
      J=Q.AddColumn 'Shipments' 'count SNO'
      J=Q.AddColumn 'TotalQuantity' 'sum QTY'
      J=Q.AddColumn 'TotalWeight' 'sum QTY * PNO.WEIGHT'
      Q.Execute 0
─────────────────────────────────────────────────────────
 ┌Supplier┐  ┌Shipments┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓S1      │  ↓6        │  ↓1,300        │  ↓19,700     │
 │S2      │  │2        │  │700          │  │10,400     │
 │S3      │  │1        │  │200          │  │3,400      │
 │S4      │  │3        │  │900          │  │12,400     │
 │S5      │  │0        │  │0            │  │0          │
 └Char(2)─┘  └Int8─────┘  └Int16────────┘  └Int16──────┘
── 5 rows by 4 columns ──────────────────────────────────
~~~

Now suppose we want to order the results by TotalQuantity. We can do this by specifying the OrderBy property:

~~~
      Q.OrderBy='TotalQuantity' 'Down'
      Q.Execute 0
─────────────────────────────────────────────────────────
 ┌Supplier┐  ┌Shipments┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓S1      │  ↓6        │  ↓1,300        │  ↓19,700     │
 │S4      │  │3        │  │900          │  │12,400     │
 │S2      │  │2        │  │700          │  │10,400     │
 │S3      │  │1        │  │200          │  │3,400      │
 │S5      │  │0        │  │0            │  │0          │
 └Char(2)─┘  └Int8─────┘  └Int16────────┘  └Int16──────┘
── 5 rows by 4 columns ──────────────────────────────────
~~~

The Having property can restrict the rows returned, and is analogous to the Where property:

~~~
      Q.Having='TotalWeight > 5000'
      Q.Execute 0
─────────────────────────────────────────────────────────
 ┌Supplier┐  ┌Shipments┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓S1      │  ↓6        │  ↓1,300        │  ↓19,700     │
 │S4      │  │3        │  │900          │  │12,400     │
 │S2      │  │2        │  │700          │  │10,400     │
 └Char(2)─┘  └Int8─────┘  └Int16────────┘  └Int16──────┘
── 3 rows by 4 columns ──────────────────────────────────
~~~

The Having property may be specified as a simple string, but can contain multiple statements, and
is always returned as a DataTable. There is an →[*.AddHaving] method that incrementally adds
statements. Like the Where property, multiple statements are logically "and-ed" together.

It is important to note that the both the Having and OrderBy properties refer to column names in
the query result, not column names in the database, though in some circumstances they may be the same.

The HavingQuota property puts an absolute cap on the number of rows returned by a grouped query:

~~~
      Q.HavingQuota=2
      Q.Execute 0
─────────────────────────────────────────────────────────
 ┌Supplier┐  ┌Shipments┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓S1      │  ↓6        │  ↓1,300        │  ↓19,700     │
 │S4      │  │3        │  │900          │  │12,400     │
 └Char(2)─┘  └Int8─────┘  └Int16────────┘  └Int16──────┘
── 2 rows by 4 columns ──────────────────────────────────
~~~

Finally, if the Having and HavingQuota properties do in fact restrict the number of rows, the rows
that are removed may be combined into a single a group and placed back in the result using the
→[*.RollUp] property:

~~~
      Q.RollUp=1
      Q.Execute 0
─────────────────────────────────────────────────────────
 ┌Supplier┐  ┌Shipments┐  ┌TotalQuantity┐  ┌TotalWeight┐
 ↓S1      │  ↓6        │  ↓1,300        │  ↓19,700     │
 │S4      │  │3        │  │900          │  │12,400     │
 │Other   │  │3        │  │900          │  │13,800     │
 └Char(5)─┘  └Int8─────┘  └Int16────────┘  └Int16──────┘
── 3 rows by 4 columns ──────────────────────────────────
~~~

Let's review the query definition that has produced this result:

~~~
      Q
── Query ────────────────────────────────────────
DatabaseName: SandP
TableName: SP
AsOf: 2
Where:
WhereNot:
WhereQuota: 0
GroupBy:  ┌Name────┐  ┌Expression┐
          ↓Supplier│  ↓SNO       │
          └Char(8)─┘  └Char(3)───┘
AllowOverlappingGroups: 0
IndependentGroups: 0
KeepEmptyGroups: 1
IncludeTotalRow: 0
Select:  ┌Name─────────┐  ┌Expression──────────┐
         ↓Shipments    │  ↓count SNO           │
         │TotalQuantity│  │sum QTY             │
         │TotalWeight  │  │sum QTY * PNO.WEIGHT│
         └Char(13)─────┘  └Char(20)────────────┘
OrderBy:  ┌Name─────────┐  ┌Direction┐
          ↓TotalQuantity│  ↓DOWN     │
          └Char(13)─────┘  └Char(4)──┘
Having:  ┌Expression────────┐
         ↓TotalWeight > 5000│
         └Char(18)──────────┘
HavingQuota: 2
RollUp: 1
─────────────────────────────────────────────────
~~~

Again, note that the order of the properties in the display is an indication of the order in which
they are logically analyzed and in which FlipDB processes them. Furthermore, note again that the
expressions in the OrderBy and Having properties are evaluated after grouping has been applied,
and thus refer to column names in the query result, rather than column names in the starting table.

