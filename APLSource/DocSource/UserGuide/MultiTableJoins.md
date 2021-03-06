# Multi-Table Joins

If is often the case that in a query, the desired column from a particular table does not directly
relate to our current or staring table, and we need to link through another table first in order
to get it. Consider the OrderDetail table from sample extended suppliers and parts database:

~~~
      d=Scripting.Server.OpenDatabase 'SandPX'
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

This table has a foreign key OrderID that refers to the Order table:

~~~
      d.GetTable 'Order'
── SandPX.Order ───────────────────────
 ┌OrderID┐  ┌SupplierID┐  ┌Date──────┐
 ↓R1     │  ↓S1        │  ↓2012-01-15│
 │R2     │  │S2        │  │2012-01-16│
 │R3     │  │S5        │  │2012-01-16│
 │R4     │  │S1        │  │2012-01-18│
 └Char(3)┘  └Char(2)───┘  └Date──────┘
── 4 rows by 3 columns ────────────────
~~~

which in turn has a foreign key SupplierID that refers to the Suppliers table:

~~~
      d.GetTable 'Supplier'
── SandPX.Supplier ─────────────────────────────
 ┌SupplierID┐  ┌Status┐  ┌City────┐  ┌Name────┐
 ↓S1        │  ↓20    │  ↓London  │  ↓Smith   │
 │S2        │  │10    │  │Paris   │  │Jones   │
 │S3        │  │30    │  │Paris   │  │Blake   │
 │S4        │  │20    │  │London  │  │Clark   │
 │S5        │  │30    │  │Athens  │  │Adams   │
 └Char(2)───┘  └Int8──┘  └Char(10)┘  └Char(10)┘
── 5 rows by 4 columns ─────────────────────────
~~~

which has yet another foreign key City that refers to the City table:

~~~
      d.GetTable 'City'
── SandPX.City ─────────
 ┌City────┐  ┌Country─┐
 ↓London  │  ↓England │
 │Paris   │  │France  │
 │Oslo    │  │Norway  │
 │Athens  │  │Greece  │
 │Rome    │  │Italy   │
 │Milan   │  │Italy   │
 └Char(10)┘  └Char(10)┘
── 6 rows by 2 columns ─
~~~

Suppose we start with a query in the OrderDetail table, grabbing a few columns:

~~~
      q=t.Query '' 'OrderID,PartID,Quantity'
      q.Execute 0
──────────────────────────────────
 ┌OrderID┐  ┌PartID─┐  ┌Quantity┐
 ↓R1     │  ↓P1     │  ↓100     │
 │R1     │  │P2     │  │200     │
 │R1     │  │P3     │  │150     │
 │R2     │  │P1     │  │75      │
 │R3     │  │P1     │  │25      │
 │R3     │  │P3     │  │125     │
 │R4     │  │P2     │  │200     │
 │R4     │  │P3     │  │300     │
 └Char(3)┘  └Char(2)┘  └Int16───┘
── 8 rows by 3 columns ───────────
~~~

Now suppose we want to know the supplier for each order detail row. We can use a simple join on the
foreign key OrderID to link to the Order table and retrieve the corresponding supplier:

~~~
       j=q.AddColumn 'SupplierID' 'OrderID.SupplierID'
       q.Execute 0
────────────────────────────────────────────────
 ┌OrderID┐  ┌PartID─┐  ┌Quantity┐  ┌SupplierID┐
 ↓R1     │  ↓P1     │  ↓100     │  ↓S1        │
 │R1     │  │P2     │  │200     │  │S1        │
 │R1     │  │P3     │  │150     │  │S1        │
 │R2     │  │P1     │  │75      │  │S2        │
 │R3     │  │P1     │  │25      │  │S5        │
 │R3     │  │P3     │  │125     │  │S5        │
 │R4     │  │P2     │  │200     │  │S1        │
 │R4     │  │P3     │  │300     │  │S1        │
 └Char(3)┘  └Char(2)┘  └Int16───┘  └Char(2)───┘
── 8 rows by 4 columns ─────────────────────────
~~~

But what if we want the City column from the Supplier table? There is no foreign key in our
starting table, OrderDetail, that points to the Supplier table. There is however a foreign key
from OrderDetail to Order, and from Order to Supplier. Thus we may use foreign keys to hop from
table to table:

~~~
      j=q.AddColumn 'City' 'OrderID.SupplierID.City'
      q.Execute 0
────────────────────────────────────────────────────────────
 ┌OrderID┐  ┌PartID─┐  ┌Quantity┐  ┌SupplierID┐  ┌City────┐
 ↓R1     │  ↓P1     │  ↓100     │  ↓S1        │  ↓London  │
 │R1     │  │P2     │  │200     │  │S1        │  │London  │
 │R1     │  │P3     │  │150     │  │S1        │  │London  │
 │R2     │  │P1     │  │75      │  │S2        │  │Paris   │
 │R3     │  │P1     │  │25      │  │S5        │  │Athens  │
 │R3     │  │P3     │  │125     │  │S5        │  │Athens  │
 │R4     │  │P2     │  │200     │  │S1        │  │London  │
 │R4     │  │P3     │  │300     │  │S1        │  │London  │
 └Char(3)┘  └Char(2)┘  └Int16───┘  └Char(2)───┘  └Char(10)┘
── 8 rows by 5 columns ─────────────────────────────────────
~~~

There is no limit to the number of tables that we may link through, so we can go all the way to the
City table and get the Country column:

~~~
      j=q.AddColumn 'Country' 'OrderID.SupplierID.City.Country'
      q.Execute 0
────────────────────────────────────────────────────────────────────────
 ┌OrderID┐  ┌PartID─┐  ┌Quantity┐  ┌SupplierID┐  ┌City────┐  ┌Country─┐
 ↓R1     │  ↓P1     │  ↓100     │  ↓S1        │  ↓London  │  ↓England │
 │R1     │  │P2     │  │200     │  │S1        │  │London  │  │England │
 │R1     │  │P3     │  │150     │  │S1        │  │London  │  │England │
 │R2     │  │P1     │  │75      │  │S2        │  │Paris   │  │France  │
 │R3     │  │P1     │  │25      │  │S5        │  │Athens  │  │Greece  │
 │R3     │  │P3     │  │125     │  │S5        │  │Athens  │  │Greece  │
 │R4     │  │P2     │  │200     │  │S1        │  │London  │  │England │
 │R4     │  │P3     │  │300     │  │S1        │  │London  │  │England │
 └Char(3)┘  └Char(2)┘  └Int16───┘  └Char(2)───┘  └Char(10)┘  └Char(10)┘
── 8 rows by 6 columns ─────────────────────────────────────────────────
~~~

We are not limited to simple joins or even one type of join when linking through multiple tables.
We can mix and match them for the desired result. Here we use general and simple joins to
accomplish the same as above:

~~~
       j=q.AddColumn 'Country2' 'Order[OrderID;OrderID].SupplierID.City[City;City].Country'
       q.Execute 0
────────────────────────────────────────────────────────────────────────────────────
 ┌OrderID┐  ┌PartID─┐  ┌Quantity┐  ┌SupplierID┐  ┌City────┐  ┌Country─┐  ┌Country2┐
 ↓R1     │  ↓P1     │  ↓100     │  ↓S1        │  ↓London  │  ↓England │  ↓England │
 │R1     │  │P2     │  │200     │  │S1        │  │London  │  │England │  │England │
 │R1     │  │P3     │  │150     │  │S1        │  │London  │  │England │  │England │
 │R2     │  │P1     │  │75      │  │S2        │  │Paris   │  │France  │  │France  │
 │R3     │  │P1     │  │25      │  │S5        │  │Athens  │  │Greece  │  │Greece  │
 │R3     │  │P3     │  │125     │  │S5        │  │Athens  │  │Greece  │  │Greece  │
 │R4     │  │P2     │  │200     │  │S1        │  │London  │  │England │  │England │
 │R4     │  │P3     │  │300     │  │S1        │  │London  │  │England │  │England │
 └Char(3)┘  └Char(2)┘  └Int16───┘  └Char(2)───┘  └Char(10)┘  └Char(10)┘  └Char(10)┘
── 8 rows by 7 columns ─────────────────────────────────────────────────────────────
~~~

