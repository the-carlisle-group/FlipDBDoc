# GatherAssignments Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method returns an array of assignment objects.

The argument is composed of 1 item:

|-|-|
|1|DataTable|DataTable|

The result is an array of assignments:

|-|-|
|1|Assignments|Array|

For example, consider the result of a query on the Suppliers table S, which yields a DataTable:

~~~
      Scripting.Server.DeleteDatabase 'SandP'
┌───────┐
│0      │
└Boolean┘
      D=Scripting.Server.BuildSuppliersAndParts ''
      T=D.GetTable 'S'
      DT=T.ExecuteQuery ''
      DT
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌SNAME───┐  ┌STATUS┐  ┌CITY────┐
 ↓S1     │  ↓Smith   │  ↓20    │  ↓London  │
 │S2     │  │Jones   │  │10    │  │Paris   │
 │S3     │  │Blake   │  │30    │  │Paris   │
 │S4     │  │Clark   │  │20    │  │London  │
 │S5     │  │Adams   │  │30    │  │Athens  │
 └Char(2)┘  └Char(10)┘  └Int8──┘  └Char(10)┘
── 5 rows by 4 columns ──────────────────────
~~~

Now make some changes to the DataTable DT:

~~~
      J=DT.SetItem 1 3 'Rome'
      J=DT.SetItem 3 2 40
      J=DT.AddRows 'S6' 'Codd' 40 'Rome'
      DT
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌SNAME───┐  ┌STATUS┐  ┌CITY────┐
 ↓S1     │  ↓Smith   │  ↓20    │  ↓London  │
 │S2     │  │Jones   │  │10    │  │Rome    │
 │S3     │  │Blake   │  │30    │  │Paris   │
 │S4     │  │Clark   │  │40    │  │London  │
 │S5     │  │Adams   │  │30    │  │Athens  │
 │S6     │  │Codd    │  │40    │  │Rome    │
 └Char(2)┘  └Char(10)┘  └Int8──┘  └Char(10)┘
── 6 rows by 4 columns ──────────────────────
      DT.Changed
┌───────┐
│1      │
└Boolean┘
~~~

The DataTable tracks changes to itself, and the GatherAssignments method may now be called to
accumulate the changes in an array of assignments:

~~~
      A=T.GatherAssignments DT
~~~

In this case, A will have two assignments in it. The first assignment handles adding new rows:

~~~
      0 pick A
── Assignment ───────────────────────────────
Table: SandP.S
Type: Add
Values:
 ┌SNO────┐  ┌SNAME───┐  ┌STATUS┐  ┌CITY────┐
 ↓S6     │  ↓Codd    │  ↓40    │  ↓Rome    │
 └Char(2)┘  └Char(10)┘  └Int8──┘  └Char(10)┘
── 1 rows by 4 columns ──────────────────────
─────────────────────────────────────────────
~~~

While the second assignment handles updating existing rows:

~~~
      1 pick A
── Assignment ───────────────────
Table: SandP.S
Type: Update
Values:
 ┌SNO────┐  ┌STATUS┐  ┌CITY────┐
 ↓S2     │  ↓10    │  ↓Rome    │
 │S4     │  │40    │  │London  │
 └Char(2)┘  └Int8──┘  └Char(10)┘
── 2 rows by 3 columns ──────────
─────────────────────────────────
~~~

The Assign method of the database may now be called to execute the multiple assignment:

~~~
      D.Assign A
┌───────┐
│0      │
└Boolean┘
      T
── SandP.S ──────────────────────────────────
 ┌SNO────┐  ┌SNAME───┐  ┌STATUS┐  ┌CITY────┐
 ↓S1     │  ↓Smith   │  ↓20    │  ↓London  │
 │S3     │  │Blake   │  │30    │  │Paris   │
 │S5     │  │Adams   │  │30    │  │Athens  │
 │S6     │  │Codd    │  │40    │  │Rome    │
 │S2     │  │Jones   │  │10    │  │Rome    │
 │S4     │  │Clark   │  │40    │  │London  │
 └Char(2)┘  └Char(10)┘  └Int8──┘  └Char(10)┘
── 6 rows by 4 columns ──────────────────────
~~~

It is clear that changes have now been saved to the Suppliers table S.  Note that executing the
GatherAssignments method does not clear the pending changes  in the DataTable:

~~~
      DT.Changed
┌───────┐
│1      │
└Boolean┘
      A2=T.GatherAssignments DT
~~~

In this case, the assignment array A2 is identical to A, the result of the first call to
GatherAssignments, as no additional changes to the DataTable have been made. If additional changes
had been made, then A2 would include those changes as well as the changes recorded in A. A side
effect of setting the Key property of a DataTable is to clear all pending changes from the
DataTable. Here the Key is reset to its existing value:

~~~
      DT.Key=DT.Key
      DT.Changed
┌───────┐
│0      │
└Boolean┘
      A3=T.GatherAssignments DT
      shape A3
┌────┐
│0   │
└Int8┘
~~~

