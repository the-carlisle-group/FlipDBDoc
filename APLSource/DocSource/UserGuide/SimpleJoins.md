# Simple Joins

Consider the shipment table SP from the classic suppliers and parts database:

~~~
      d=Scripting.Server.OpenDatabase 'SandP'
      t=d.GetTable 'SP'
      t
── SandP.SP ────────────────────────────────────────────
 ┌AUTOKEY┐  ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌SDATE─────┐
 ↓1      │  ↓S1     │  ↓P1     │  ↓300  │  ↓2008-10-27│
 │2      │  │S1     │  │P2     │  │200  │  │2008-08-19│
 │3      │  │S1     │  │P3     │  │400  │  │2008-05-18│
 │4      │  │S1     │  │P4     │  │200  │  │2007-08-18│
 │5      │  │S1     │  │P5     │  │100  │  │2007-08-17│
 │6      │  │S1     │  │P6     │  │100  │  │2009-12-16│
 │7      │  │S2     │  │P1     │  │300  │  │2009-07-31│
 │8      │  │S2     │  │P2     │  │400  │  │2009-12-17│
 │9      │  │S3     │  │P2     │  │200  │  │2009-07-24│
 │10     │  │S4     │  │P2     │  │200  │  │2009-10-03│
 │11     │  │S4     │  │P4     │  │300  │  │2009-06-28│
 │12     │  │S4     │  │P5     │  │400  │  │2009-11-04│
 └Int8───┘  └Char(2)┘  └Char(2)┘  └Int16┘  └Date──────┘
── 12 rows by 5 columns ────────────────────────────────
~~~

Now suppose we want to create a report that lists all of the rows of this table but also shows the
city in which the supplier SNO is located. It is clear the supplier's city is not in this table,
but rather is to be found in the supplier table S:

~~~
      d.GetTable 'S'
── SandP.S ──────────────────────────────────
 ┌SNO────┐  ┌SNAME───┐  ┌STATUS┐  ┌CITY────┐
 ↓S1     │  ↓Smith   │  ↓20    │  ↓London  │
 │S2     │  │Jones   │  │10    │  │Paris   │
 │S3     │  │Blake   │  │30    │  │Paris   │
 │S4     │  │Clark   │  │20    │  │London  │
 │S5     │  │Adams   │  │30    │  │Athens  │
 └Char(2)┘  └Char(10)┘  └Int8──┘  └Char(10)┘
── 5 rows by 4 columns ──────────────────────
~~~

By inspection, we can see that for each row in the SP table, we need to look up the supplier number
in the supplier table and find the corresponding city, and use that to create a new column in the
SP table.

Let's create a new query on the SP table, selecting the supplier number, part number, and quantity columns:

~~~
      q=t.Query '' 'SNO,PNO,QTY'
~~~

Now, we need to somehow get the CITY column from the suppliers table S conceptually into the table
SP. We do this by "dotting" from the foreign key column SNO into the suppliers table and
specifying the CITY column as follows:

~~~
       j=q.AddColumn 'City' 'SNO.CITY'
~~~

Let's execute and display the results of the query:

~~~
       q.Execute 0
───────────────────────────────────────────
 ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌City────┐
 ↓S1     │  ↓P1     │  ↓300  │  ↓London  │
 │S1     │  │P2     │  │200  │  │London  │
 │S1     │  │P3     │  │400  │  │London  │
 │S1     │  │P4     │  │200  │  │London  │
 │S1     │  │P5     │  │100  │  │London  │
 │S1     │  │P6     │  │100  │  │London  │
 │S2     │  │P1     │  │300  │  │Paris   │
 │S2     │  │P2     │  │400  │  │Paris   │
 │S3     │  │P2     │  │200  │  │Paris   │
 │S4     │  │P2     │  │200  │  │London  │
 │S4     │  │P4     │  │300  │  │London  │
 │S4     │  │P5     │  │400  │  │London  │
 └Char(2)┘  └Char(2)┘  └Int16┘  └Char(10)┘
── 12 rows by 4 columns ───────────────────
~~~

And review how we got here by inspecting the Select property:

~~~
        q.Select
────────────────────────────────────
 ┌Name───┐  ┌Expression┐  ┌Visible┐
 ↓SNO    │  ↓SNO       │  ↓1      │
 │PNO    │  │PNO       │  │1      │
 │QTY    │  │QTY       │  │1      │
 │City   │  │SNO.CITY  │  │1      │
 └Char(4)┘  └Char(8)───┘  └Boolean┘
── 4 rows by 3 columns ─────────────
~~~

Note that in the expression for the City result column, SNO refers to a column in the starting
table SP and that this column is a foreign key. As a foreign key, it refers to the primary key of
another table, namely the suppliers table S. Thus the foreign key SNO implies the table S, and
thus the table S need not be explicitly referenced. Not only does the foreign key imply another
table, but it also implies a specific many-to-one relationship between the rows of the starting
table and the rows of the foreign table. In this case we know that for every row in SP there must
be one corresponding row in the suppliers table S. Therefore, the expression SNO.CITY fully
specifies how the CITY column in the suppliers table S is to be represented in the SP table.

In FlipDB this type of join is called a "simple join" and is one the most common ways to bring in a
column from another table.

We can of course use this simple join syntax anywhere in query, not just in the select property.
For example, to find all shipments from suppliers whose status is 20:

~~~
      t.ExecuteQuery 'SNO.STATUS eq 20' 'SNO,PNO,QTY'
───────────────────────────────
 ┌SNO────┐  ┌PNO────┐  ┌QTY──┐
 ↓S1     │  ↓P1     │  ↓300  │
 │S1     │  │P2     │  │200  │
 │S1     │  │P3     │  │400  │
 │S1     │  │P4     │  │200  │
 │S1     │  │P5     │  │100  │
 │S1     │  │P6     │  │100  │
 │S4     │  │P2     │  │200  │
 │S4     │  │P4     │  │300  │
 │S4     │  │P5     │  │400  │
 └Char(2)┘  └Char(2)┘  └Int16┘
── 9 rows by 3 columns ────────
~~~

And to group the parts by color and count the number of shipments we can dot into the parts table P
via the foreign key PNO:

~~~
      q=t.Query '' ''
      q.GroupBy='Color' 'PNO.COLOR'
      j=q.AddColumn 'Count' 'count PNO'
      j=q.AddColumn 'TotalQuantity' 'sum QTY'
      q.Execute 0
─────────────────────────────────────
 ┌Color──┐  ┌Count┐  ┌TotalQuantity┐
 ↓Red    │  ↓5    │  ↓1,200        │
 │Green  │  │4    │  │1,000        │
 │Blue   │  │3    │  │900          │
 └Char(5)┘  └Int8─┘  └Int16────────┘
── 3 rows by 3 columns ──────────────
~~~

