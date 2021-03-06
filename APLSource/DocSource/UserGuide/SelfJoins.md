# Self Joins

The general join syntax allows for self-joins, where the foreign table is the same as the starting
table. One useful application of self-join is when a where clause is applied, and the join in
question is an enclosed join. This allows us to reach outside the confines of the where clause and
look at the entire table. Consider a query on the shipments table, applying a where clause to
restrict the rows to only supplier S1:

~~~
      t=d.GetTable 'SP'
      q=t.Query 'SNO in "S1"' 'PNO,QTY'
      q.Execute 0
───────────────────────
 ┌PNO────┐  ┌QTY──┐
 ↓P1     │  ↓300  │
 │P2     │  │200  │
 │P3     │  │400  │
 │P4     │  │200  │
 │P5     │  │100  │
 │P6     │  │100  │
 └Char(2)┘  └Int16┘
── 6 rows by 2 columns
~~~

When a query is evaluated, the where clause is applied first, and from that point on the query only
knows about the selected rows. Thus, in the result above, the QTY column only shows the 6 values
for supplier S1.  However, a join normally steps into another table and in this the new table the
where clause is not directly applicable. When we use a self-join to step out and back into the
same table we get the entire starting table, not just the rows specified by the where clause. So,
adding a self-join back to the SP table for the QTY column gives us both the quantities for the
selected rows, and the quantities for all rows:

~~~
           j=q.AddColumn 'AllQTY' 'SP[].QTY'
           q.Execute 0
─────────────────────────────────────────────────────────────────────────
 ┌PNO────┐  ┌QTY──┐  ┌AllQTY───────────────────────────────────────────┐
 ↓P1     │  ↓300  │  │[300,200,400,200,100,100,300,400,200,200,300,400]│
 │P2     │  │200  │  └Int16────────────────────────────────────────────┘
 │P3     │  │400  │
 │P4     │  │200  │
 │P5     │  │100  │
 │P6     │  │100  │
 └Char(2)┘  └Int16┘
── 6 rows by 3 columns ──────────────────────────────────────────────────
~~~

This is a very useful result. For example, we can get percentages with respect to the entire table,
not just the selected rows:

