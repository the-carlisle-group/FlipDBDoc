# General Joins

Simple joins, partitioned joins, and time series joins, all require pre-defined foreign key
relationships, while enclosed joins require no relationship at all. All of these specific joins
are in fact special cases of a "general" join which allows us to join tables on arbitrary columns
or sets of columns. In a general join we must explicitly specify the foreign table and the
lookup-columns in both the starting table and the foreign table; nothing is implied.

To see how general joins work, let's look at a simple join and the equivalent general join. First,
we will open up the suppliers and parts database:

~~~
      d=Scripting.Server.OpenDatabase 'SandP'
~~~

Then extract the shipments table:

~~~
      t=d.GetTable 'SP'
~~~

and then create a query, selecting a few columns:

~~~
      q=t.Query '' 'SNO,PNO,QTY'
~~~

Now we will get the CITY column from the suppliers table using simple join syntax by hopping from
the foreign key SNO:

~~~
      j=q.AddColumn 'City1' 'SNO.CITY'
~~~

And then get the CITY column again using general join syntax:

~~~
      j=q.AddColumn 'City2' 'S[SNO;SNO].CITY'
~~~

Now let's execute the query:

~~~
      q.Execute 0
───────────────────────────────────────────────────────
 ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌City1───┐  ┌City2───┐
 ↓S1     │  ↓P1     │  ↓300  │  ↓London  │  ↓London  │
 │S1     │  │P2     │  │200  │  │London  │  │London  │
 │S1     │  │P3     │  │400  │  │London  │  │London  │
 │S1     │  │P4     │  │200  │  │London  │  │London  │
 │S1     │  │P5     │  │100  │  │London  │  │London  │
 │S1     │  │P6     │  │100  │  │London  │  │London  │
 │S2     │  │P1     │  │300  │  │Paris   │  │Paris   │
 │S2     │  │P2     │  │400  │  │Paris   │  │Paris   │
 │S3     │  │P2     │  │200  │  │Paris   │  │Paris   │
 │S4     │  │P2     │  │200  │  │London  │  │London  │
 │S4     │  │P4     │  │300  │  │London  │  │London  │
 │S4     │  │P5     │  │400  │  │London  │  │London  │
 └Char(2)┘  └Char(2)┘  └Int16┘  └Char(10)┘  └Char(10)┘
── 12 rows by 5 columns ───────────────────────────────
~~~

and review the Select property:

~~~
      q.Select
─────────────────────────────────────────
 ┌Name───┐  ┌Expression─────┐  ┌Visible┐
 ↓SNO    │  ↓SNO            │  ↓1      │
 │PNO    │  │PNO            │  │1      │
 │QTY    │  │QTY            │  │1      │
 │City1  │  │SNO.CITY       │  │1      │
 │City2  │  │S[SNO;SNO].CITY│  │1      │
 └Char(5)┘  └Char(15)───────┘  └Boolean┘
── 5 rows by 3 columns ──────────────────
~~~

In the general join expression for City2, we must explicitly specify the foreign table S as well as
the lookup key SNO. In fact, the lookup key must be specified twice: once for the starting table
(the shipments tables SP) and once for the foreign table (the suppliers table S). This is because
in a general join there is no requirement that the lookup key columns have the same name in both
tables. If the names do happen to be the same, then we can elide the repetition. However, we must
still provide the semicolon. For example, we can add column City3, which has an identical result
to City1 and City2:

~~~
      j=q.AddColumn 'City3' 'S[SNO;].CITY'
      q.Execute 0
───────────────────────────────────────────────────────────────────
 ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌City1───┐  ┌City2───┐  ┌City3───┐
 ↓S1     │  ↓P1     │  ↓300  │  ↓London  │  ↓London  │  ↓London  │
 │S1     │  │P2     │  │200  │  │London  │  │London  │  │London  │
 │S1     │  │P3     │  │400  │  │London  │  │London  │  │London  │
 │S1     │  │P4     │  │200  │  │London  │  │London  │  │London  │
 │S1     │  │P5     │  │100  │  │London  │  │London  │  │London  │
 │S1     │  │P6     │  │100  │  │London  │  │London  │  │London  │
 │S2     │  │P1     │  │300  │  │Paris   │  │Paris   │  │Paris   │
 │S2     │  │P2     │  │400  │  │Paris   │  │Paris   │  │Paris   │
 │S3     │  │P2     │  │200  │  │Paris   │  │Paris   │  │Paris   │
 │S4     │  │P2     │  │200  │  │London  │  │London  │  │London  │
 │S4     │  │P4     │  │300  │  │London  │  │London  │  │London  │
 │S4     │  │P5     │  │400  │  │London  │  │London  │  │London  │
 └Char(2)┘  └Char(2)┘  └Int16┘  └Char(10)┘  └Char(10)┘  └Char(10)┘
── 12 rows by 6 columns ───────────────────────────────────────────
~~~

Now let's examine a partitioned join and its equivalent general join, using a query from the
suppliers table:

~~~
      t=d.GetTable 'S'
      q=t.Query '' 'SNO'
      j=q.AddColumn 'Quantity1' 'SP[SNO].QTY'
      j=q.AddColumn 'Quantity2' 'SP[SNO;SNO].QTY'
      q.Execute 0
── Key:SNO ──────────────────────────────────────────────────────────
 ┌SNO────┐  ┌Quantity1────────────────┐  ┌Quantity2────────────────┐
 ↓S1     │  ↓[300,200,400,200,100,100]│  ↓[300,200,400,200,100,100]│
 │S2     │  │[300,400]                │  │[300,400]                │
 │S3     │  │[200]                    │  │[200]                    │
 │S4     │  │[200,300,400]            │  │[200,300,400]            │
 │S5     │  │[]                       │  │[]                       │
 └Char(2)┘  └Int16────────────────────┘  └Int16────────────────────┘
── 5 rows by 3 columns ──────────────────────────────────────────────
~~~

Quantity1 is computed using a partitioned join, as SNO is foreign key in SP pointing back the
suppliers table S. In the expression for Quantity2 we use general join syntax, explicitly
specifying the look-up column SNO in both the S and the SP table. The results are identical in
this case.

Note that a general join will return either a simple column or a partitioned column depending on
the actual relationship of the current data, regardless of the relationship implied by the
specified lookup columns. While a primary/foreign key relationship implies one-to-many (or zero),
it does not forbid one-to-one. If, in the case above, we happened to have an exact one-to-one
relationship between S and SP, then Quantity 1 would be a partitioned column containing one item
in each group, but Quantity 2 would be simple column.

Now let's take a look at an enclosed join and its equivalent general join. An enclosed join, you
will remember, has no relevant relationship; we simply bring in the entire column as-is from the
foreign table with no re-ordering, replication or partitioning. The result, however, is enclosed,
rather than being left simple. Consider a query from the suppliers table, bringing in the entire
part number column from the parts table. We will bring in the column using both enclosed join
syntax and general join syntax:

~~~
      t=d.GetTable 'S'
      q=t.Query '' 'SNO,SNAME'
      j=q.AddColumn 'Parts1' 'P[].PNO'
      j=q.AddColumn 'Parts2' 'P[;].PNO'
      q.Execute 0
── Key:SNO ──────────────────────────────────────────────────────────
 ┌SNO────┐  ┌SNAME───┐  ┌Parts1─────────────┐  ┌Parts2─────────────┐
 ↓S1     │  ↓Smith   │  │[P1,P2,P3,P4,P5,P6]│  │[P1,P2,P3,P4,P5,P6]│
 │S2     │  │Jones   │  └Char(2)────────────┘  └Char(2)────────────┘
 │S3     │  │Blake   │
 │S4     │  │Clark   │
 │S5     │  │Adams   │
 └Char(2)┘  └Char(10)┘
── 5 rows by 4 columns ──────────────────────────────────────────────
~~~

The general join syntax allows for empty column name lists, and this produces an identical result
to the enclosed join syntax.

General joins can handle many-to-many relationships as well. Consider the OrderDetail table from
the extended suppliers and parts database:

~~~
       d=Scripting.Server.OpenDatabase'SandPX'
       d.GetTable 'OrderDetail'
── SandPX.OrderDetail ───────────────────────
 ┌AUTOKEY┐  ┌OrderID┐  ┌PartID─┐  ┌Quantity┐
 ↓1      │  ↓R1     │  ↓P1     │  ↓100     │
 │2      │  │R1     │  │P2     │  │200     │
 │3      │  │R1     │  │P3     │  │150     │
 │4      │  │R2     │  │P1     │  │75      │
 │5      │  │R3     │  │P1     │  │25      │
 │6      │  │R3     │  │P3     │  │125     │
 │7      │  │R4     │  │P2     │  │200     │
 │8      │  │R4     │  │P3     │  │300     │
 └Int8───┘  └Char(3)┘  └Char(2)┘  └Int16───┘
── 8 rows by 4 columns ──────────────────────
~~~

...and the historical Price table as well:

~~~
       d.GetTable 'Price'
── SandPX.Price ────────────────────────────────────────────
 ┌AUTOKEY┐  ┌PartID─┐  ┌AsOfDate──┐  ┌Price─┐  ┌DiscountID┐
 ↓1      │  ↓P1     │  ↓2012-01-15│  ↓1.15  │  ↓A         │
 │2      │  │P2     │  │2012-01-15│  │3.25  │  │A         │
 │3      │  │P3     │  │2012-01-15│  │2.75  │  │A         │
 │4      │  │P1     │  │2012-01-16│  │1.18  │  │B         │
 │5      │  │P2     │  │2012-01-16│  │3.15  │  │B         │
 │6      │  │P3     │  │2012-01-16│  │2.80  │  │A         │
 │7      │  │P1     │  │2012-01-17│  │1.25  │  │A         │
 │8      │  │P2     │  │2012-01-17│  │3.05  │  │B         │
 │9      │  │P3     │  │2012-01-17│  │2.75  │  │C         │
 │10     │  │P1     │  │2012-01-18│  │1.35  │  │B         │
 │11     │  │P2     │  │2012-01-18│  │2.95  │  │C         │
 │12     │  │P3     │  │2012-01-18│  │2.80  │  │C         │
 │13     │  │P1     │  │2012-01-19│  │1.40  │  │B         │
 │14     │  │P3     │  │2012-01-20│  │2.70  │  │A         │
 └Int8───┘  └Char(2)┘  └Date──────┘  └Dec(2)┘  └Char(1)───┘
── 14 rows by 5 columns ────────────────────────────────────
~~~

PartID is not unique in either table, and thus we have many-to-many relationship. Now consider a
query on the OrderDetail table, selecting a few columns and then bringing in the Price column from
the Price table by joining on PartID:

~~~
      t=d.GetTable 'OrderDetail'
      q=t.Query '' 'OrderID,PartID'
      j=q.AddColumn 'AllPrices' 'Price[PartID;PartID].Price'
      q.Execute 0
────────────────────────────────────────────────────
 ┌OrderID┐  ┌PartID─┐  ┌AllPrices─────────────────┐
 ↓R1     │  ↓P1     │  ↓[1.15,1.18,1.25,1.35,1.40]│
 │R1     │  │P2     │  │[3.25,3.15,3.05,2.95]     │
 │R1     │  │P3     │  │[2.75,2.80,2.75,2.80,2.70]│
 │R2     │  │P1     │  │[1.15,1.18,1.25,1.35,1.40]│
 │R3     │  │P1     │  │[1.15,1.18,1.25,1.35,1.40]│
 │R3     │  │P3     │  │[2.75,2.80,2.75,2.80,2.70]│
 │R4     │  │P2     │  │[3.25,3.15,3.05,2.95]     │
 │R4     │  │P3     │  │[2.75,2.80,2.75,2.80,2.70]│
 └Char(3)┘  └Char(2)┘  └Dec(2)────────────────────┘
── 8 rows by 3 columns ─────────────────────────────
~~~

We see that all prices for part P1 in the Price table are grouped and associated with each
occurrence of P1 in the  OrderDetail table. Like a partitioned join, the column values in the
linked table are grouped by the lookup key, but like a simple join, these groups are ordered and
replicated to line up with the starting table.

Again, it is common practice to apply an aggregate function to partitioned join results. Here, an
average might be appropriate:

~~~
      j=q.AddColumn 'AveragePrice' '2 round average AllPrices'
      q.Execute 0
────────────────────────────────────────────────────────────────────
 ┌OrderID┐  ┌PartID─┐  ┌AllPrices─────────────────┐  ┌AveragePrice┐
 ↓R1     │  ↓P1     │  ↓[1.15,1.18,1.25,1.35,1.40]│  ↓1.27        │
 │R1     │  │P2     │  │[3.25,3.15,3.05,2.95]     │  │3.10        │
 │R1     │  │P3     │  │[2.75,2.80,2.75,2.80,2.70]│  │2.76        │
 │R2     │  │P1     │  │[1.15,1.18,1.25,1.35,1.40]│  │1.27        │
 │R3     │  │P1     │  │[1.15,1.18,1.25,1.35,1.40]│  │1.27        │
 │R3     │  │P3     │  │[2.75,2.80,2.75,2.80,2.70]│  │2.76        │
 │R4     │  │P2     │  │[3.25,3.15,3.05,2.95]     │  │3.10        │
 │R4     │  │P3     │  │[2.75,2.80,2.75,2.80,2.70]│  │2.76        │
 └Char(3)┘  └Char(2)┘  └Dec(2)────────────────────┘  └Dec(2)──────┘
── 8 rows by 4 columns ─────────────────────────────────────────────
~~~

