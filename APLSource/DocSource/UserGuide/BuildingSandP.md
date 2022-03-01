# Building the Suppliers and Parts Database

While the Server has a built-in method for creating the suppliers and parts database, it is useful
to see how it is created step by step, exercising the basic methods of the →[*.Objects.Database]
and →[*.Objects.Table] objects. First, delete the existing copy of SandP, and create a new
database using the →[*.MethodList.CreateDatabase] method:

~~~
      S=Scripting.Server
      J=S.DeleteDatabase 'SandP'
      D=S.CreateDatabase 'SandP'
      D.TableNames
┌───────┐
↓Char(0)┘
~~~

D is now a new database with no tables. The →[*.MethodList.CreateTable] method creates a new
table, and returns the table as its result. We can create the suppliers table:

~~~
      ST=D.CreateTable 'S'
      ST
── SandP.S ────────────
 ┌AUTOKEY┐
 ↓Int8───┘
── 0 rows by 1 columns
~~~

New tables are created with no rows, and 1 column. This column is a system column, AUTOKEY, and is
the default primary key of the table. AUTOKEY is an auto-incrementing integer primary key. The
→[*.MethodList.CreatePrimaryKey] method creates a new column and designates it as the primary key
(This method only works on "new" tables, that is, tables that have never had any rows added to them):

~~~
      ST.CreatePrimaryKey 'SNO' 'Char(2)'
┌SNO────┐
↓Char(2)┘
~~~

If the suppliers table is now displayed again, note that the AUTOKEY column has disappeared:

~~~
      ST
── SandP.S ────────────
 ┌SNO────┐
 ↓Char(2)┘
── 0 rows by 1 columns
~~~

Designating a column other than AUTOKEY to be the primary key causes the AUTOKEY column to be in
accessible. Now create the supplier name column:

~~~
      J=ST.CreateColumn 'SNAME' 'Char(10)'
~~~

And use the →[*.MethodList.SetAltKey] method to specify that SNAME is an alternate key:

~~~
      J=ST.SetAltKey 'SNAME'
~~~

Note that creating an alternate key column is a two-step process. The column must be created first,
and then specified as an alternate key. This is because alternate keys may be compound, and
contain any number of columns. The remaining columns are created with:

~~~
      J=ST.CreateColumn 'STATUS' 'Int32'
      J=ST.CreateColumn 'CITY' 'Char(10)'
~~~

And now the suppliers table has the desired structure, but has no data in it:

~~~
      ST
── SandP.S ──────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌CITY──────┐
 ↓Char(2)┘  ↓Char(10)──┘  ↓Int8──┘  ↓Char(10)──┘
── 0 rows by 4 columns ──────────────────────────
~~~

The parts table is created in a similar manner:

~~~
      PT=D.CreateTable'P'
      J=PT.CreatePrimaryKey'PNO' 'Char(2)'
      J=PT.CreateColumn'PNAME' 'Char(5)'
      J=PT.CreateColumn'COLOR' 'Char(5)'
      J=PT.CreateColumn'WEIGHT' 'Int32'
      J=PT.CreateColumn'CITY' 'Char(10)'
~~~

Finally, the suppliers and parts cross reference table is created:

~~~
      SP=D.CreateTable 'SP'
~~~

The →[*.MethodList.CreateForeignKey] method is used to create a foreign key. Its argument is
composed of two items, the name of the column, and the name of the foreign table to which it refers:

~~~
      J=SP.CreateForeignKey'SNO' 'S'
~~~

Note that there is no need to specify a type, as a foreign key always points to the primary key of
the foreign table, and thus the type of a foreign key column is the same as the type of the
primary key of the foreign table. Note further that this method only works on new, unpopulated
tables. Populated columns may be specified as foreign keys using the
→[*.MethodList.SetForeignKey] method. The part number column is also a foreign key, pointing to
the parts table:

~~~
      J=SP.CreateForeignKey'PNO' 'P'
~~~

The SNO and PNO columns should together form a compound key. Version 1.0 of FlipDB does not support
composite primary keys, so we make them an alternate key:

~~~
      J=SP.SetAltKey'SNO' 'PNO'
~~~

Finally, we create the remaining columns for parts quantity and ship date:

~~~
      J=SP.CreateColumn'QTY' 'Int32'
      J=SP.CreateColumn'SDATE' 'Date'
~~~

We have now created the complete suppliers and parts database, though it remains unpopulated. In
order to populate a table, it is useful to have a DataTable that contains the data that will be
used to insert or update the Table:

~~~
      DT=Scripting.DataTable.New ''
~~~

One way to add data to a DataTable is column by column:

~~~
      DT.AddColumn 'SNO' 'S1,S2,S3,S4,S5'
───────────────────────
 ┌SNO────┐
 ↓S1     │
 │S2     │
 │S3     │
 │S4     │
 │S5     │
 └Char(2)┘
── 5 rows by 1 columns
~~~

The →[*.DataTable.MethodList.AddColumn] method returns the same DataTable instance. In order to
suppress the display in the session as the remaining columns are added, we can assign the result
back to DT (or to any temporary name):

~~~
      DT=DT.AddColumn 'SNAME' 'Smith,Jones,Blake,Clark,Adams'
      DT=DT.AddColumn 'STATUS' (20 10 30 20 30)
      DT=DT.AddColumn 'CITY' 'London,Paris,Paris,London,Athens'
~~~

Let's review the assembled data:

~~~
      DT
───────────────────────────────────────────
 ┌SNO────┐  ┌SNAME──┐  ┌STATUS┐  ┌CITY───┐
 ↓S1     │  ↓Smith  │  ↓20    │  ↓London │
 │S2     │  │Jones  │  │10    │  │Paris  │
 │S3     │  │Blake  │  │30    │  │Paris  │
 │S4     │  │Clark  │  │20    │  │London │
 │S5     │  │Adams  │  │30    │  │Athens │
 └Char(2)┘  └Char(5)┘  └Int8──┘  └Char(6)┘
── 5 rows by 4 columns ────────────────────
~~~

And now use the →[*.MethodList.Insert] method to add it to the suppliers table:

~~~
      J=ST.Insert DT
      ST
── SandP.S ──────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌CITY──────┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓London    │
 │S2     │  │Jones     │  │10    │  │Paris     │
 │S3     │  │Blake     │  │30    │  │Paris     │
 │S4     │  │Clark     │  │20    │  │London    │
 │S5     │  │Adams     │  │30    │  │Athens    │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Char(10)──┘
── 5 rows by 4 columns ──────────────────────────
~~~

Repeat the procedure for the parts table by first building the DataTable:

~~~
      DT=Scripting.DataTable.New ''
      DT=DT.AddColumn 'PNO' 'P1,P2,P3,P4,P5,P6'
      DT=DT.AddColumn 'PNAME' 'Nut,Bolt,Screw,Screw,Cam,Cog'
      DT=DT.AddColumn 'COLOR' 'Red,Green,Blue,Red,Blue,Red'
      DT=DT.AddColumn 'WEIGHT' (12 17 17 14 12 19)
      DT=DT.AddColumn 'CITY' 'London,Paris,Oslo,London,Paris,London'
~~~

and then actually inserting the rows into the parts table:

~~~
      J=PT.Insert DT
      PT
── SandP.P ──────────────────────────────────────────────
 ┌PNO────┐  ┌PNAME──┐  ┌COLOR──┐  ┌WEIGHT┐  ┌CITY──────┐
 ↓P1     │  ↓Nut    │  ↓Red    │  ↓12    │  ↓London    │
 │P2     │  │Bolt   │  │Green  │  │17    │  │Paris     │
 │P3     │  │Screw  │  │Blue   │  │17    │  │Oslo      │
 │P4     │  │Screw  │  │Red    │  │14    │  │London    │
 │P5     │  │Cam    │  │Blue   │  │12    │  │Paris     │
 │P6     │  │Cog    │  │Red    │  │19    │  │London    │
 └Char(2)┘  └Char(5)┘  └Char(5)┘  └Int8──┘  └Char(10)──┘
── 6 rows by 5 columns ──────────────────────────────────
~~~

And finally for the suppliers and parts cross reference table

~~~
      DT=Scripting.DataTable.New ''
      DT=DT.AddColumn  'SNO' (6 2 1 3 replicate 'S1,S2,S3,S4')
      DT=DT.AddColumn 'PNO' (catenate 'P' '1,2,3,4,5,6,1,2,2,2,4,5')
      DT=DT.AddColumn 'QTY' (100 * 3 2 4 2 1 1 3 4 2 2 3 4)
      J=SP.Insert DT
      SP
── SandP.SP ────────────────────────────────────────────
 ┌AUTOKEY┐  ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌SDATE─────┐
 ↓ 1     │  ↓S1     │  ↓P1     │  ↓300  │  ↓         0│
 │ 2     │  │S1     │  │P2     │  │200  │  │         0│
 │ 3     │  │S1     │  │P3     │  │400  │  │         0│
 │ 4     │  │S1     │  │P4     │  │200  │  │         0│
 │ 5     │  │S1     │  │P5     │  │100  │  │         0│
 │ 6     │  │S1     │  │P6     │  │100  │  │         0│
 │ 7     │  │S2     │  │P1     │  │300  │  │         0│
 │ 8     │  │S2     │  │P2     │  │400  │  │         0│
 │ 9     │  │S3     │  │P2     │  │200  │  │         0│
 │10     │  │S4     │  │P2     │  │200  │  │         0│
 │11     │  │S4     │  │P4     │  │300  │  │         0│
 │12     │  │S4     │  │P5     │  │400  │  │         0│
 └Int8───┘  └Char(2)┘  └Char(2)┘  └Int16┘  └Date──────┘
── 12 rows by 5 columns ────────────────────────────────
~~~

It is left as an exercise for the reader to fill in the supplier date column.

