# Partitioned Joins

We have seen how a simple join is used to bring in a column from a short table based on a foreign
key. Items from the column in the short table are replicated and ordered to line up with the long
table. This process may be reversed, and we may start a query in the short table and bring in a
column from the long table. In this case, the items from the column are sorted and partitioned to
line up with the small table. To see how this works, let's review the suppliers table S:

~~~
      d=Scripting.Server.OpenDatabase 'SandP'
      t=d.GetTable 'S'
      t
── SandP.S ──────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐  ┌SNAME───┐
 ↓S1     │  ↓20    │  ↓London  │  ↓Smith   │
 │S2     │  │10    │  │Paris   │  │Jones   │
 │S3     │  │30    │  │Paris   │  │Blake   │
 │S4     │  │20    │  │London  │  │Clark   │
 │S5     │  │30    │  │Athens  │  │Adams   │
 └Char(2)┘  └Int8──┘  └Char(10)┘  └Char(10)┘
── 5 rows by 4 columns ──────────────────────
~~~

and the shipment table SP:

~~~
      d.GetTable 'SP'
── SandP.SP ────────────────────────────────────────────
 ┌AUTOKEY┐  ┌QTY──┐  ┌SDATE─────┐  ┌SNO────┐  ┌PNO────┐
 ↓1      │  ↓300  │  ↓2008-10-27│  ↓S1     │  ↓P1     │
 │2      │  │200  │  │2008-08-19│  │S1     │  │P2     │
 │3      │  │400  │  │2008-05-18│  │S1     │  │P3     │
 │4      │  │200  │  │2007-08-18│  │S1     │  │P4     │
 │5      │  │100  │  │2007-08-17│  │S1     │  │P5     │
 │6      │  │100  │  │2009-12-16│  │S1     │  │P6     │
 │7      │  │300  │  │2009-07-31│  │S2     │  │P1     │
 │8      │  │400  │  │2009-12-17│  │S2     │  │P2     │
 │9      │  │200  │  │2009-07-24│  │S3     │  │P2     │
 │10     │  │200  │  │2009-10-03│  │S4     │  │P2     │
 │11     │  │300  │  │2009-06-28│  │S4     │  │P4     │
 │12     │  │400  │  │2009-11-04│  │S4     │  │P5     │
 └Int8───┘  └Int16┘  └Date──────┘  └Char(2)┘  └Char(2)┘
── 12 rows by 5 columns ────────────────────────────────
~~~

Remember that SP has a foreign key SNO, which refers to the primary key of the suppliers table S,
which is also named SNO. Now we will create a query from the suppliers table, selecting a few columns:

~~~
       q=t.Query '' 'SNO,STATUS,CITY,SNAME'
~~~

And then add the quantity column from the suppliers and part shipment table SP, using partition
join syntax:

~~~
      j=q.AddColumn 'Quantity' 'SP[SNO].QTY'
~~~

Now let's execute the query:

~~~
      q.Execute 0
── Key:SNO ───────────────────────────────────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐  ┌SNAME───┐  ┌Quantity─────────────────┐
 ↓S1     │  ↓20    │  ↓London  │  ↓Smith   │  ↓[300,200,400,200,100,100]│
 │S2     │  │10    │  │Paris   │  │Jones   │  │[300,400]                │
 │S3     │  │30    │  │Paris   │  │Blake   │  │[200]                    │
 │S4     │  │20    │  │London  │  │Clark   │  │[200,300,400]            │
 │S5     │  │30    │  │Athens  │  │Adams   │  │[]                       │
 └Char(2)┘  └Int8──┘  └Char(10)┘  └Char(10)┘  └Int16────────────────────┘
── 5 rows by 5 columns ───────────────────────────────────────────────────
~~~

...and review the select property:

~~~
      q.Select
──────────────────────────────────────
 ┌Name────┐  ┌Expression─┐  ┌Visible┐
 ↓SNO     │  ↓SNO        │  ↓1      │
 │STATUS  │  │STATUS     │  │1      │
 │CITY    │  │CITY       │  │1      │
 │SNAME   │  │SNAME      │  │1      │
 │Quantity│  │SP[SNO].QTY│  │1      │
 └Char(8)─┘  └Char(11)───┘  └Boolean┘
── 5 rows by 3 columns ───────────────
~~~

The first thing to notice about the Quantity column in the result is that it is a partitioned
column; the QTY column from able SP has been partitioned and ordered to correspond to the supplier
table. This is necessary as the relationship between S and SP is one-to-many. Note further that
the last item in Quantity is empty, as there are no shipments from supplier S5 in the table SP.

Let's examine the partition join syntax a little closer. We may generalize it as:

~~~
      T[FK].C
~~~

where T is a table, FK is a foreign key column in T that refers back to the starting table, and C
is any column in T. Note that unlike the simple join syntax FK.C, it is necessary to explicitly
specify the table T. Note also that it is necessary to specify the foreign key as there may be
more than one foreign key in T that refers back to the starting table.

Because the result of a partitioned join is a partitioned column, it is common practice to apply an
aggregate function, yielding a simple column. For example, rather than returning the partitioned
column Quantity as above, we may return the shipment count and the total number of parts shipped
per supplier as follows:

~~~
      q=t.Query '' 'SNO,SNAME'
      j=q.AddColumn 'ShipmentCount' 'count SP[SNO].QTY'
      j=q.AddColumn 'TotalQuantity' 'sum SP[SNO].QTY'
      q.Execute 0
── Key:SNO ──────────────────────────────────────────────
 ┌SNO────┐  ┌SNAME───┐  ┌ShipmentCount┐  ┌TotalQuantity┐
 ↓S1     │  ↓Smith   │  ↓6            │  ↓1,300        │
 │S2     │  │Jones   │  │2            │  │700          │
 │S3     │  │Blake   │  │1            │  │200          │
 │S4     │  │Clark   │  │3            │  │900          │
 │S5     │  │Adams   │  │0            │  │0            │
 └Char(2)┘  └Char(10)┘  └Int8─────────┘  └Int16────────┘
── 5 rows by 4 columns ──────────────────────────────────
~~~

...and once again review the select property:

~~~
      q.Select
─────────────────────────────────────────────────
 ┌Name─────────┐  ┌Expression───────┐  ┌Visible┐
 ↓SNO          │  ↓SNO              │  ↓1      │
 │SNAME        │  │SNAME            │  │1      │
 │ShipmentCount│  │count SP[SNO].QTY│  │1      │
 │TotalQuantity│  │sum SP[SNO].QTY  │  │1      │
 └Char(13)─────┘  └Char(17)─────────┘  └Boolean┘
── 4 rows by 3 columns ──────────────────────────
~~~

When we look at this query result, it is very close, if not identical to a grouped query on the
shipment table. This reflects the inverse nature of the simple join and the partition join. For
comparison, let's create a query in table SP:

~~~
      q=(d.GetTable 'SP').Query '' ''
~~~

...grouping by the supplier name in the suppliers table, using a simple join on the foreign key SNO:

~~~
       q.GroupBy='SupplierName' 'SNO.SNAME'
~~~

And then add the shipment count and total quantity statistics:

~~~
      j=q.AddColumn 'Count' 'count SNO'
      j=q.AddColumn 'TotalQTY' 'sum QTY'
~~~

Now execute the query:

~~~
      q.Execute 0
─────────────────────────────────────
 ┌SupplierName┐  ┌Count┐  ┌TotalQTY┐
 ↓Smith       │  ↓6    │  ↓1,300   │
 │Jones       │  │2    │  │700     │
 │Blake       │  │1    │  │200     │
 │Clark       │  │3    │  │900     │
 └Char(10)────┘  └Int8─┘  └Int16───┘
── 4 rows by 3 columns ──────────────
~~~

This result is almost identical to the previous query. The most obvious difference is that there is
no entry for Adams because there are no shipments from supplier S5. However, if we group by the
foreign key directly, the supplier number, then we can actually can get the zero row in our result
as well:

~~~
      q.GroupBy='SNO'
      q.Execute 0
───────────────────────────────────
 ┌GroupBySNO┐  ┌Count┐  ┌TotalQTY┐
 ↓S1        │  ↓6    │  ↓1,300   │
 │S2        │  │2    │  │700     │
 │S3        │  │1    │  │200     │
 │S4        │  │3    │  │900     │
 │S5        │  │0    │  │0       │
 └Char(2)───┘  └Int8─┘  └Int16───┘
── 5 rows by 3 columns ────────────
~~~

In the special case of grouping by a foreign key, the query result will contain "zero rows" if the
KeepEmptyGroups property of the query is set to 1, which is the default.

While it is generally possible to produce a given query result either by doing an ungrouped query
in the short table and doing partition joins, or by doing a grouped query in the long table and
doing simple joins, in practice one technique will usually be a much better choice. If the query
result is mostly made up of summary statistics from the long table, then a grouped query on the
long table is easier, while if the query result is mostly unsummarized rows from the short table
then an ungrouped query from the short table is easier.

Partition joins are also useful when performing grouped queries. Consider a query on the suppliers
table, grouped by CITY:

~~~
      q=(d.GetTable 'S').Query ''
      q.GroupBy='City' 'CITY'
      j=  q.AddColumn 'Count' 'count SNO'
      j= q.AddColumn 'Quantity' 'SP[SNO].QTY'
      q.Execute 0
──────────────────────────────────────────────────────────────
 ┌City────┐  ┌Count┐  ┌Quantity─────────────────────────────┐
 ↓London  │  ↓2    │  ↓[300,200,400,200,100,100,200,300,400]│
 │Paris   │  │2    │  │[300,400,200]                        │
 │Athens  │  │1    │  │[]                                   │
 └Char(10)┘  └Int8─┘  └Int16────────────────────────────────┘
── 3 rows by 3 columns ───────────────────────────────────────
~~~

Here the QTY column has been partitioned by supplier number, and then these partitions have been
grouped together by City. Alternatively, we may think of this as grouping the suppliers by CITY,
and then combining all the shipments for all the suppliers of particular CITY. For example, the
Paris group consists of suppliers S2 and S3. S2 has two shipments for quantities 300 and 400,
while S3 has one shipment for 200. These three QTY items are then combined together.

