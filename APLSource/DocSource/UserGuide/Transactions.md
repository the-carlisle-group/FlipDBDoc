# Transactions

A transaction is an atomic unit of work that changes the state of the database. Any insert, update
or delete operation on a table is a transaction. FlipDB tracks transactions, and assigns a
transaction ID to each transaction. Transaction ID's are integers that start at 1. Consider the
suppliers and parts database, and, as usual, make a fresh copy of it:

~~~
      S=Scripting.Server
      J=S.DeleteDatabase 'SandP'
      D=S.BuildSuppliersAndParts ''
~~~

Every table has a system column named TWID (transaction wrap identifier). While the TWID column
does not show up in the list of column names it may be specified in any query:

~~~
      ST=D.GetTable 'S'
      ST.ExecuteQuery '' 'SNO,SNAME,STATUS,TWID'
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌TWID┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓1   │
 │S2     │  │Jones     │  │10    │  │1   │
 │S3     │  │Blake     │  │30    │  │1   │
 │S4     │  │Clark     │  │20    │  │1   │
 │S5     │  │Adams     │  │30    │  │1   │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Int8┘
── 5 rows by 4 columns ────────────────────
~~~

We see that all of the suppliers were added to the supplier table in one transaction, and that it
was the first change to the database, thus the TWID is 1. Now let's add a couple of new suppliers,
and review the table:

~~~
      J=ST.Insert ('SNO' 'S6,S7') ('SNAME' 'Codd,Date')
      ST.ExecuteQuery '' 'SNO,SNAME,STATUS,TWID'
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌TWID┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓1   │
 │S2     │  │Jones     │  │10    │  │1   │
 │S3     │  │Blake     │  │30    │  │1   │
 │S4     │  │Clark     │  │20    │  │1   │
 │S5     │  │Adams     │  │30    │  │1   │
 │S6     │  │Codd      │  │ 0    │  │4   │
 │S7     │  │Date      │  │ 0    │  │4   │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Int8┘
── 7 rows by 4 columns ────────────────────
~~~

This change to the database has been assigned a transaction number of 4. We can infer from this
that there must be 2 previous transactions, and in fact the parts table and the suppliers table
are each populated by the BuildSuppliersAndParts method in one transaction, accounting for TWIDs 2
and 3. Now let's update some existing rows:

~~~
      J=ST.Update ('SNO' 'S3,S4,S6') ('STATUS' (70 80 90))
      ST.ExecuteQuery '' 'SNO,SNAME,STATUS,TWID'
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌TWID┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓1   │
 │S2     │  │Jones     │  │10    │  │1   │
 │S5     │  │Adams     │  │30    │  │1   │
 │S7     │  │Date      │  │ 0    │  │4   │
 │S3     │  │Blake     │  │70    │  │5   │
 │S4     │  │Clark     │  │80    │  │5   │
 │S6     │  │Codd      │  │90    │  │5   │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Int8┘
── 7 rows by 4 columns ────────────────────
~~~

This transaction is assigned 5, the next available transaction number. Note that the query returns
the results ordered by TWID. Tables are naturally ordered by TWID, and unless a query has been
specifically sorted, the result will be in transaction order. This means that rows newly added or
updated rows will be at the bottom of the table.

There terms "insert", "update" and "delete", are shorthand for the more formal term "assignment."
In the above examples, the transactions have been made up of single assignments. Each transaction
has been a single insert or update operation to a single table. (These are considered single
assignments, even though multiple rows may be updated or inserted.) In FlipDB a transaction may
consist of more than one assignment. This allows multiple insert, update, or delete operations to
one or more tables to be bundled into a single, atomic, unit of work. In addition to the suppliers
table, consider the parts and the suppliers and parts cross reference tables:

~~~
      PT=D.GetTable 'P'
      PT.ExecuteQuery '' 'PNO,PNAME,WEIGHT,TWID'
────────────────────────────────────────
 ┌PNO────┐  ┌PNAME──┐  ┌WEIGHT┐  ┌TWID┐
 ↓P1     │  ↓Nut    │  ↓12    │  ↓2   │
 │P2     │  │Bolt   │  │17    │  │2   │
 │P3     │  │Screw  │  │17    │  │2   │
 │P4     │  │Screw  │  │14    │  │2   │
 │P5     │  │Cam    │  │12    │  │2   │
 │P6     │  │Cog    │  │19    │  │2   │
 └Char(2)┘  └Char(5)┘  └Int8──┘  └Int8┘
 ── 6 rows by 4 columns ─────────────────
      SP=D.GetTable 'SP'
      SP.ExecuteQuery '' 'PNO,SNO,QTY,TWID'
───────────────────────────────────────
 ┌PNO────┐  ┌SNO────┐  ┌QTY──┐  ┌TWID┐
 ↓P1     │  ↓S1     │  ↓300  │  ↓3   │
 │P2     │  │S1     │  │200  │  │3   │
 │P3     │  │S1     │  │400  │  │3   │
 │P4     │  │S1     │  │200  │  │3   │
 │P5     │  │S1     │  │100  │  │3   │
 │P6     │  │S1     │  │100  │  │3   │
 │P1     │  │S2     │  │300  │  │3   │
 │P2     │  │S2     │  │400  │  │3   │
 │P2     │  │S3     │  │200  │  │3   │
 │P2     │  │S4     │  │200  │  │3   │
 │P4     │  │S4     │  │300  │  │3   │
 │P5     │  │S4     │  │400  │  │3   │
 └Char(2)┘  └Char(2)┘  └Int16┘  └Int8┘
── 12 rows by 4 columns ───────────────
~~~

The Table object provides three methods →[*.DeferredInsert], →[*.DeferredUpdate], and
→[*.DeferredDelete], that exactly correspond in syntax to the →[*.MethodList.Insert],
→[*.MethodList.Update], and →[*.Table.MethodList.Delete] methods. However, instead of performing
the operation immediately, they return an Assignment object that can subsequently be gathered with
additional assignment objects and executed in one single transaction. For example:

~~~
      A1=ST.DeferredUpdate ('SNO' 'S7') ('STATUS' 50)
      A2=PT.DeferredInsert ('PNO' 'P7') ('PNAME' 'Nail')
      A3=SP.DeferredInsert ('PNO' 'P7,P7') ('SNO' 'S1,S7')('QTY' (500 600))
~~~

Here we have accumulated 3 assignments, A1, A2, and A3, involving prospective changes to three
different tables. The assignment A1 will change the status of supplier S7 to 50, the assignment A2
will add a new part number P7 to the parts table and the assignment A3 will add two new rows to
the cross reference table. Note that it would not be possible to execute assignment A3 on its own,
given the current state of the database, because part P7 does not exist, and the PNO column in the
cross reference table is a foreign key and indeed FlipDB will reject such an assignment:

~~~
      SP.Insert ('PNO' 'P8,P8') ('SNO' 'S1,S7')('QTY' (500 600))
Referential integrity violation. (PNO)
~~~

It must be emphasized that assignments A1, A2, and A3 represent prospective changes; No changes
have yet been made to the database. The →[*.MethodList.Assign] method executes the assignments,
and changes the state of the database:

~~~
      J=D.Assign A1 A2 A3
~~~

A review of the supplier table shows that in transaction number 6, supplier S7's status has changed
to 50:

~~~
      ST.ExecuteQuery '' 'SNO,SNAME,STATUS,TWID'
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌TWID┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓1   │
 │S2     │  │Jones     │  │10    │  │1   │
 │S5     │  │Adams     │  │30    │  │1   │
 │S3     │  │Blake     │  │70    │  │5   │
 │S4     │  │Clark     │  │80    │  │5   │
 │S6     │  │Codd      │  │90    │  │5   │
 │S7     │  │Date      │  │50    │  │6   │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Int8┘
── 7 rows by 4 columns ────────────────────
~~~

While a review of the parts table shows that a new part has been added, also in transaction number 6:

~~~
      PT.ExecuteQuery '' 'PNO,PNAME,WEIGHT,TWID'
────────────────────────────────────────
 ┌PNO────┐  ┌PNAME──┐  ┌WEIGHT┐  ┌TWID┐
 ↓P1     │  ↓Nut    │  ↓12    │  ↓2   │
 │P2     │  │Bolt   │  │17    │  │2   │
 │P3     │  │Screw  │  │17    │  │2   │
 │P4     │  │Screw  │  │14    │  │2   │
 │P5     │  │Cam    │  │12    │  │2   │
 │P6     │  │Cog    │  │19    │  │2   │
 │P7     │  │Nail   │  │ 0    │  │6   │
 └Char(2)┘  └Char(5)┘  └Int8──┘  └Int8┘
── 7 rows by 4 columns ─────────────────
~~~

Finally, transaction number 6 has added two new rows to the cross reference table:

~~~
      SP.ExecuteQuery '' 'PNO,SNO,QTY,TWID'
───────────────────────────────────────
 ┌PNO────┐  ┌SNO────┐  ┌QTY──┐  ┌TWID┐
 ↓P1     │  ↓S1     │  ↓300  │  ↓3   │
 │P2     │  │S1     │  │200  │  │3   │
 │P3     │  │S1     │  │400  │  │3   │
 │P4     │  │S1     │  │200  │  │3   │
 │P5     │  │S1     │  │100  │  │3   │
 │P6     │  │S1     │  │100  │  │3   │
 │P1     │  │S2     │  │300  │  │3   │
 │P2     │  │S2     │  │400  │  │3   │
 │P2     │  │S3     │  │200  │  │3   │
 │P2     │  │S4     │  │200  │  │3   │
 │P4     │  │S4     │  │300  │  │3   │
 │P5     │  │S4     │  │400  │  │3   │
 │P7     │  │S1     │  │500  │  │6   │
 │P7     │  │S7     │  │600  │  │6   │
 └Char(2)┘  └Char(2)┘  └Int16┘  └Int8┘
── 14 rows by 4 columns ───────────────
~~~

FlipDB guarantees that a transaction is atomic; the transaction either completes successfully, or
has absolutely no effect. It is not possible for one of its component assignments to execute,
while another fails. This is true for failure of any type, whether it is due to a power failure in
the middle of a transaction or to a violation of database constraints.

A transaction represents a change from one state to another in a database. The change may affect
one or more tables. FlipDB provides built-in support for inspecting the database as it appeared in
the past, that is, previous states of the database. A database has as many states as there are
TWIDs. In the above example, the suppliers and parts database has 6 states: 1 current state and 5
past states. The →[*.Query.MethodList.Execute|Query.Execute] method allows you to query past
states. Consider a query on the supplier table:

~~~
      Q=ST.Query ''
      J=Q.AddColumns 'SNO,SNAME,STATUS,TWID'
      Q.Execute 0
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌TWID┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓1   │
 │S2     │  │Jones     │  │10    │  │1   │
 │S5     │  │Adams     │  │30    │  │1   │
 │S3     │  │Blake     │  │70    │  │5   │
 │S4     │  │Clark     │  │80    │  │5   │
 │S6     │  │Codd      │  │90    │  │5   │
 │S7     │  │Date      │  │50    │  │6   │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Int8┘
── 7 rows by 4 columns ────────────────────
~~~

When the argument to Execute is 0, the current state of the database is queried. If the argument is-
, the database is queried N states ago. Thus we can look back at the suppliers table before the
last transaction:

~~~
      Q.Execute -1
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌TWID┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓1   │
 │S2     │  │Jones     │  │10    │  │1   │
 │S5     │  │Adams     │  │30    │  │1   │
 │S7     │  │Date      │  │ 0    │  │4   │
 │S3     │  │Blake     │  │70    │  │5   │
 │S4     │  │Clark     │  │80    │  │5   │
 │S6     │  │Codd      │  │90    │  │5   │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Int8┘
── 7 rows by 4 columns ────────────────────
~~~

and two transactions ago:

~~~
      Q.Execute -2
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌TWID┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓1   │
 │S2     │  │Jones     │  │10    │  │1   │
 │S3     │  │Blake     │  │30    │  │1   │
 │S4     │  │Clark     │  │20    │  │1   │
 │S5     │  │Adams     │  │30    │  │1   │
 │S6     │  │Codd      │  │ 0    │  │4   │
 │S7     │  │Date      │  │ 0    │  │4   │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Int8┘
~~~

and three transactions ago:

~~~
      Q.Execute -3
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌TWID┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓1   │
 │S2     │  │Jones     │  │10    │  │1   │
 │S3     │  │Blake     │  │30    │  │1   │
 │S4     │  │Clark     │  │20    │  │1   │
 │S5     │  │Adams     │  │30    │  │1   │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Int8┘
── 5 rows by 4 columns ────────────────────
~~~

Note that applying a where clause to the TWID column is not equivalent to querying a past state.
For example, restricting a query on the current state to TWIDs equal to 1 will not produce the
table as it appeared at state 1. In fact, it returns only those rows in the suppliers table that
have remained unchanged since state 1, perhaps a useful query:

~~~
      ST.ExecuteQuery 'TWID == 1' ''
─────────────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌CITY──────┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓London    │
 │S2     │  │Jones     │  │10    │  │Paris     │
 │S5     │  │Adams     │  │30    │  │Athens    │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Char(10)──┘
── 3 rows by 4 columns ──────────────────────────
~~~

The Execute method will accept a DateTime as an argument, querying the database as of a specific
point in time. This feature is useful for producing month-end, quarter-end and year-end reports.
The parts table will appear empty at the turn of the last century:

~~~
      Q.Execute '1999/12/31 23:59:59'
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌TWID┐
 ↓Char(2)┘  ↓Char(10)──┘  ↓Int8──┘  ↓Int8┘
── 0 rows by 4 columns ────────────────────
~~~

The Execute method can also query the history of rows in a table. It is most useful when
restricting the result to a single primary key value:

~~~
      Q.Where='SNO in "S4"'
      Q.Execute 1
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌TWID┐
 ↓S4     │  ↓Clark     │  ↓20    │  ↓1   │
 │S4     │  │Clark     │  │80    │  │5   │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Int8┘
── 2 rows by 4 columns ────────────────────
~~~

This shows the complete history of changes to supplier S4. To the extent that a row has been
updated, a history query will return multiple rows per primary key. History queries show data as
it changes over time.

