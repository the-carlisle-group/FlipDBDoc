# Databases, Tables, and Columns

The →[*.Objects.Server] object contains →[*.Objects.Database] objects, which in turn contain
→[*.Objects.Table] objects, which in turn contain →[*.Objects.Column] objects. There is one and
only one instance of the server, and it is accessed under the →[*.Objects.Scripting] object:

~~~
      S=Scripting.Server
~~~

The Server object is used to create, access, and delete databases. The
→[*.MethodList.BuildSuppliersAndParts] method creates the well-known supplier and parts database
used in the writings of C.J Data and E.F. Codd. This database is used in examples throughout the
user guide and reference manual. Before creating it, ensure that it does not exists by deleting it
using the →[*.MethodList.DeleteDatabase] method:

~~~
       J=S.DeleteDatabase 'SandP'
~~~

The assignment of the result to the variable J is simply to prevent the unneeded result of the
method (a zero in this case) from displaying in the session. You will see this technique
throughout the user guide. Now re-create the suppliers and parts database:

~~~
      D=S.BuildSuppliersAndParts ''
~~~

This method returns a database object, and by convention we assign it to the variable D.

## Databases

The Database object is used to create, access and delete tables. The →[*.Database.Properties.Name]
property returns the name of the database, and the →[*.Database.Properties.TableNames] property
returns the names of the tables in the database:

~~~
      D.Name
┌───────┐
│SandP  │
└Char(5)┘
      D.TableNames
┌───────┐
↓S      │
│P      │
│SP     │
└Char(2)┘
~~~

A Table is extracted using the →[*.Database.MethodList.GetTable] method:

~~~
      ST=D.GetTable 'S'
      ST
── SandP.S ──────────────────────────────────────
 ┌SNO────┐  ┌STATUS┐  ┌CITY──────┐  ┌SNAME─────┐
 ↓S1     │  ↓20    │  ↓London    │  ↓Smith     │
 │S2     │  │10    │  │Paris     │  │Jones     │
 │S3     │  │30    │  │Paris     │  │Blake     │
 │S4     │  │20    │  │London    │  │Clark     │
 │S5     │  │30    │  │Athens    │  │Adams     │
 └Char(2)┘  └Int8──┘  └Char(10)──┘  └Char(10)──┘
── 5 rows by 4 columns ──────────────────────────
~~~

This is the suppliers table S. Note that the name of the table is "S", while ST is simply a name we
have given to the instance of the table object that contains the table S. The suppliers table
contains 5 suppliers. The column SNO, supplier number, is the primary key, and by definition
cannot contain duplicate values. Supplier S1 is named Smith, has status 20, and is located in
London. Now extract the parts table P:

~~~
      PT=D.GetTable 'P'
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

The parts table contains data on 6 parts. For example, part number P2 is a bolt, is green, of
weight 17, and is located in a warehouse in Paris. The suppliers and parts cross reference table
SP records the parts that each supplier has supplied:

~~~
      SP=D.GetTable 'SP'
      SP
── SandP.SP ────────────────────────────────────────────
 ┌AUTOKEY┐  ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌SDATE─────┐
 ↓ 1     │  ↓S1     │  ↓P1     │  ↓300  │  ↓2008-10-27│
 │ 2     │  │S1     │  │P2     │  │200  │  │2008-08-19│
 │ 3     │  │S1     │  │P3     │  │400  │  │2008-05-18│
 │ 4     │  │S1     │  │P4     │  │200  │  │2007-08-18│
 │ 5     │  │S1     │  │P5     │  │100  │  │2007-08-17│
 │ 6     │  │S1     │  │P6     │  │100  │  │2009-12-16│
 │ 7     │  │S2     │  │P1     │  │300  │  │2009-07-31│
 │ 8     │  │S2     │  │P2     │  │400  │  │2009-12-17│
 │ 9     │  │S3     │  │P2     │  │200  │  │2009-07-24│
 │10     │  │S4     │  │P2     │  │200  │  │2009-10-03│
 │11     │  │S4     │  │P4     │  │300  │  │2009-06-28│
 │12     │  │S4     │  │P5     │  │400  │  │2009-11-04│
 └Int8───┘  └Char(2)┘  └Char(2)┘  └Int16┘  └Date──────┘
── 12 rows by 5 columns ────────────────────────────────
~~~

For example, supplier S2 has supplied 300 units of part P1, and 400 units of P2. These 3 tables
make up the suppliers and parts database.

## Tables

The Table object is used to create, access, and delete columns. The
→[*.Table.Properties.ColumnNames] property returns the names of the columns in the table:

~~~
      ST.ColumnNames
┌───────┐
↓SNO    │
│SNAME  │
│STATUS │
│CITY   │
└Char(6)┘
~~~

A column is extracted from a table using the →[*.Table.MethodList.GetColumn] method:

~~~
      C=ST.GetColumn 'STATUS'
      C
┌STATUS┐
↓20    │
│10    │
│30    │
│20    │
│30    │
└Int8──┘
~~~

The →[*.Column.Properties.Type] property returns the data type of a column:

~~~
     C.Type
┌───────┐
│Int32  │
└Char(5)┘
~~~

Note that a Column object has properties and methods, while a DataColumn does not. A Column object
can be used in place of a DataColumn. This means we can use functions on Column objects:

~~~
      sum SP.GetColumn 'QTY'
┌─────┐
│3100 │
└Int16┘
~~~

A Column may be thought of as a permanent, named DataColumn. Similarly, a Table may be thought of
as a permanent, named DataTable. The Database, Table, and Column objects represent persistent
entities. Changes to them are immediately saved. For example, the →[*.ChangeType] method may be
used to increase the storage size of a column:

~~~
      C.ChangeType 'Char(32)'
┌CITY────────────────────────────┐
↓London                          │
│Paris                           │
│Paris                           │
│London                          │
│Athens                          │
└Char(32)────────────────────────┘
~~~

The →[*.MethodList.CreateColumn] method will add a new column to the table:

~~~
      ST.CreateColumn 'Country' 'Char(16)'
┌Country─────────┐
↓                │
│                │
│                │
│                │
│                │
└Char(16)────────┘
~~~

## Valid Names

Database, Table, and Column names must conform to the following rules. Names must begin with an
uppercase letter from A to Z. The rest of the name may be composed of uppercase or lowercase
letters from A to Z and digits 0 to 9. The maximum length of a name is 128 characters.

Note that these naming rules apply to the permanent, persistent objects themselves and not to
references to the objects that one might use in the session or a script.

