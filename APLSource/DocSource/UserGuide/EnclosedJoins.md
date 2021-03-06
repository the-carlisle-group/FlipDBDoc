# Enclosed Joins

We have seen that simple joins and partition joins both depend on a foreign key relationship
between two tables. An enclosed joins allows us to bring in an entire column from another table
with no regard for keys, and with no ordering, selecting or replicating. This type of join can be
surprisingly useful.

Consider a query on the supplier table once again.

~~~
      d=Scripting.Server.OpenDatabase 'SandP'
      t=d.GetTable 'S'
      q=t.Query '' 'SNO,CITY,SNAME'
      q.Execute 0
── Key:SNO ────────────────────────
 ┌SNO────┐  ┌CITY────┐  ┌SNAME───┐
 ↓S1     │  ↓London  │  ↓Smith   │
 │S2     │  │Paris   │  │Jones   │
 │S3     │  │Paris   │  │Blake   │
 │S4     │  │London  │  │Clark   │
 │S5     │  │Athens  │  │Adams   │
 └Char(2)┘  └Char(10)┘  └Char(10)┘
── 5 rows by 3 columns ────────────
~~~

Now let's bring in the SNO column from the shipment table into the query using an enclosed join:

~~~
      j=q.AddColumn 'ShipmentSNO' 'SP[].SNO'
~~~

...and execute the query:

~~~
      q.Execute 0
── Key:SNO ─────────────────────────────────────────────────────────────────
 ┌SNO────┐  ┌CITY────┐  ┌SNAME───┐  ┌ShipmentSNO──────────────────────────┐
 ↓S1     │  ↓London  │  ↓Smith   │  │[S1,S1,S1,S1,S1,S1,S2,S2,S3,S4,S4,S4]│
 │S2     │  │Paris   │  │Jones   │  └Char(2)──────────────────────────────┘
 │S3     │  │Paris   │  │Blake   │
 │S4     │  │London  │  │Clark   │
 │S5     │  │Athens  │  │Adams   │
 └Char(2)┘  └Char(10)┘  └Char(10)┘
── 5 rows by 4 columns ─────────────────────────────────────────────────────
~~~

...and review the select property:

~~~
      q.Select
────────────────────────────────────────
 ┌Name───────┐  ┌Expression┐  ┌Visible┐
 ↓SNO        │  ↓SNO       │  ↓1      │
 │CITY       │  │CITY      │  │1      │
 │SNAME      │  │SNAME     │  │1      │
 │ShipmentSNO│  │SP[].SNO  │  │1      │
 └Char(11)───┘  └Char(8)───┘  └Boolean┘
── 4 rows by 3 columns ─────────────────
~~~

An enclosed join produces a column in the query result that has no direct relationship to the rest
of the result; it does not line up in any meaningful way. In the case above, there are 12 rows in
the SP table, and only 5 rows in the suppliers table S. When a column from SP is brought in to S,
their respective items do not correspond with each other. For this reason, the simple column SNO
in table SP is enclosed. Visually, in the FlipDB session, the fact that it is enclosed helps us
see that ShipmentSNO is a somewhat arbitrary (from the point of view of table S) list of supplier
numbers. If ShipmentSNO was not enclosed, and left as a simple column, the query would result in
an irregular DataTable, with columns of different length.

Of what use, then, is an enclosed join? Suppose we want a list of suppliers who supplied no
shipments at all. By using the enclosed join syntax in a where clause, we can easily answer this question:

~~~
      t.ExecuteQuery 'not SNO in SP[].SNO' ''
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐  ┌SNAME───┐
 ↓S5     │  ↓30    │  ↓Athens  │  ↓Adams   │
 └Char(2)┘  └Int8──┘  └Char(10)┘  └Char(10)┘
── 1 rows by 4 columns ──────────────────────
~~~

This use of an enclosed join as the right argument to the "in" function is quite common. In this
particular case, because SNO is a foreign key in table SP, an alternative solution is to use a
partitioned join:

~~~
      t.ExecuteQuery '0 eq count SP[SNO].SNO' ''
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐  ┌SNAME───┐
 ↓S5     │  ↓30    │  ↓Athens  │  ↓Adams   │
 └Char(2)┘  └Int8──┘  └Char(10)┘  └Char(10)┘
── 1 rows by 4 columns ──────────────────────
~~~

The intent of the first technique is arguably clearer. Regardless, it is often the case that it is
impractical, for one reason or another, to specify keys. We often are faced with two sets of data
arranged in tables, and we need to identify rows in one table that exist or do not exist in
another. The enclosed join and the "in" function are a perfect solution.

Let's look at another query where enclosed join may be useful. Consider the problem of finding
suppliers who supply all parts. We can use an enclosed join to get a list of all parts from the
parts table, and then compare this to the list of parts supplied by each supplier:

~~~
      t.ExecuteQuery 'all P[].PNO in each SP[SNO].PNO' ''
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐  ┌SNAME───┐
 ↓S1     │  ↓20    │  ↓London  │  ↓Smith   │
 └Char(2)┘  └Int8──┘  └Char(10)┘  └Char(10)┘
── 1 rows by 4 columns ──────────────────────
~~~

It turns out that it is only Smith who suppliers all 5 parts. Let's unpack this where statement and
see how the result is achieved. The enclosed join to the parts table gives a list of all parts,
enclosed of course, as the list has no direct relationship to the suppliers table where the query starts:

~~~
      ap=t.Evaluate 'P[].PNO'
      ap
┌───────────────────┐
│[P1,P2,P3,P4,P5,P6]│
└Char(2)────────────┘
~~~

The partitioned join to the SP table gives the parts supplied by each supplier. The result is
partitioned, and corresponds row by row with the suppliers table:

~~~
       ps=t.Evaluate 'SP[SNO].PNO'
       ps
┌───────────────────┐
↓[P1,P2,P3,P4,P5,P6]│
│[P1,P2]            │
│[P2]               │
│[P2,P4,P5]         │
│[]                 │
└Char(2)────────────┘
~~~

We can now compare each item in the all parts list (ap) with each item the parts supplied list (ps):

~~~
       ap in each ps
┌─────────────┐
↓[1,1,1,1,1,1]│
│[1,1,0,0,0,0]│
│[0,1,0,0,0,0]│
│[0,1,0,1,1,0]│
│[0,0,0,0,0,0]│
└Boolean──────┘
~~~

This produces a partitioned boolean column, flagging the parts that are supplied by each supplier.
We can now apply the aggregate function "all", to see who supplies all parts:

~~~
       all ap in each ps
┌───────┐
↓1      │
│0      │
│0      │
│0      │
│0      │
└Boolean┘
~~~

And finally we see that this corresponds with supplier S1:

~~~
       (all ap in each ps) (t.Evaluate 'SNO')
 ┌───────┐  ┌───────┐
 ↓1      │  ↓S1     │
 │0      │  │S2     │
 │0      │  │S3     │
 │0      │  │S4     │
 │0      │  │S5     │
 └Boolean┘  └Char(2)┘
~~~

Yet another use of enclosed joins is to report across multiple tables as if they were  first
combined in one large table, and then grouped by the source table. Consider the two tables S and
P. Say we want a query result that shows the two table names, the number of rows in each table,
and perhaps some other information from each table. We can start this query from an arbitrary
table in the database:

~~~
      q=t.Query ''
~~~

The first column in the result is constructed manually, by simply listing the table names in the
order we would like them to appear:

~~~
      j=q.AddColumn 'Table' '"Supplier,Parts"'
~~~

To compute the number of rows in each table, we use an enclosed join on any column in each table,
making sure to reference the tables in the same order that we list them in the first result column:

~~~
      j=q.AddColumn 'Rows' 'count enclose S[].SNO P[].PNO'
~~~

Note that the enclose function is used to convert two or more separate columns, which coming from
different tables may be of different length, into a single partitioned column. We then apply the
aggregate count function on this partitioned column.

Similarly we may get the supplier name and part name columns from the suppliers and parts table
respectively, and use the aggregate function toCSV to make a simple list from each partition:

~~~
      j=q.AddColumn 'Names' 'toCSV enclose S[].SNAME P[].PNAME'
~~~

Note that when combining columns from multiple tables in this way, there is no need for the column
names to match. It is only necessary that the columns have compatible types.

Finally, consider computing the total from the WEIGHT column in the parts table. There is no
corresponding column in the suppliers table, but we may use a zero as a place holder:

~~~
      j=q.AddColumn 'TotalWeight' 'sum enclose 0 P[].WEIGHT'
~~~

Now let's execute and see the results:

~~~
      q.Execute 0
────────────────────────────────────────────────────────────────────
 ┌Table───┐  ┌Rows┐  ┌Names────────────────────────┐  ┌TotalWeight┐
 ↓Supplier│  ↓5   │  ↓Smith,Jones,Blake,Clark,Adams│  ↓0          │
 │Parts   │  │6   │  │Nut,Bolt,Screw,Screw,Cam,Cog │  │91         │
 └Char(8)─┘  └Int8┘  └Char(29)─────────────────────┘  └Int8───────┘
── 2 rows by 4 columns ─────────────────────────────────────────────
~~~

By inspection it should be clear that this result is equivalent to a grouped query on the union of
the two tables, where a column indicating the source table is added (on which we do the grouping),
and other columns, like SNAME and PNAME, are renamed to a common name. Combining tables for the
purposes of a query can be accomplished in a script, and in some cases that may be the best
choice, but being able to do this in a single, simple query is extremely useful.

As the number of tables grows and the computations become more extensive, as they certainly will in
a real example, it is useful to define the partitioned columns first, setting the visible property
to 0, and then subsequently apply the aggregate functions. This is good both for readability and
performance. For example, the above query may be rewritten as:

~~~
      q=t.Query ''
      j=q.AddColumn 'Table' '"Supplier,Parts"'
      j=q.AddColumn 'ID' 'enclose S[].SNO P[].PNO' 0
      j=q.AddColumn 'Name' 'enclose S[].SNAME P[].PNAME' 0
      j=q.AddColumn 'Weight' 'enclose 0 P[].WEIGHT' 0
      j=q.AddColumn 'Rows' 'count ID'
      j=q.AddColumn 'Names' 'toCSV Name'
      j=q.AddColumn 'TotalWeight' 'sum Weight'
~~~

Let's review the Select property we have just created:

~~~
      q.Select
─────────────────────────────────────────────────────────
 ┌Name───────┐  ┌Expression─────────────────┐  ┌Visible┐
 ↓Table      │  ↓"Supplier,Parts"           │  ↓1      │
 │ID         │  │enclose S[].SNO P[].PNO    │  │0      │
 │Name       │  │enclose S[].SNAME P[].PNAME│  │0      │
 │Weight     │  │enclose 0 P[].WEIGHT       │  │0      │
 │Rows       │  │count ID                   │  │1      │
 │Names      │  │toCSV Name                 │  │1      │
 │TotalWeight│  │sum Weight                 │  │1      │
 └Char(11)───┘  └Char(27)───────────────────┘  └Boolean┘
── 7 rows by 3 columns ──────────────────────────────────
~~~

And now execute the query and see the exact same result:

~~~
      q.Execute 0
────────────────────────────────────────────────────────────────────
 ┌Table───┐  ┌Rows┐  ┌Names────────────────────────┐  ┌TotalWeight┐
 ↓Supplier│  ↓5   │  ↓Smith,Jones,Blake,Clark,Adams│  ↓0          │
 │Parts   │  │6   │  │Nut,Bolt,Screw,Screw,Cam,Cog │  │91         │
 └Char(8)─┘  └Int8┘  └Char(29)─────────────────────┘  └Int8───────┘
── 2 rows by 4 columns ─────────────────────────────────────────────
~~~

This technique of querying across multiple tables does not obviate the need for good schema design.
If multiple tables in fact contain information on the same type of entity, they should be combined
into one table if possible. One obvious downside of maintaining multiple tables is that if a new
table is added all of the above expressions in the select clause need to be modified to include
the new table, whereas no changes to the query would be needed in a single table scenario.
However, there are many cases where tables have very few columns in common and should not be
combined in the schema, or other constraints simply prohibit combining tables. In these cases,
using enclosed joins is a simple and powerful solution to a difficult problem.

