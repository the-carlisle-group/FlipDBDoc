# Keys

→[*.Objects.Table] objects have one or more columns that are designated as primary, alternate, or
foreign keys. Primary and alternate keys establish identity and enforce uniqueness on the rows of
a table. Foreign keys establish a relationship between two tables, and enforce referential integrity.

## Primary Keys

Every table has a single column designated as its primary key. FlipDB enforces uniqueness on the
primary key; No two rows in a table may have the same primary key value. A primary key column may
be of any data type except Blob. When a table is first created it has a primary key column of type
Int32 named AUTOKEY:

~~~
      D=Scripting.Server.OpenDatabase 'SandP'
      T=D.CreateTable 'Employee'
      T
── SandP.Employee ─────
 ┌AUTOKEY┐
 ↓Int8───┘
── 0 rows by 1 columns
~~~

The AUTOKEY column, as its name implies, contains an automatically generated key value starting at
1, and incremented by 1 for each new row added to the table:

~~~
      T.CreateColumn 'LastName' 'Char(16)'
┌LastName────────┐
↓Char(16)────────┘
      T.CreateColumn 'FirstName' 'Char(16)'
┌FirstName───────┐
↓Char(16)────────┘
       T.Insert ('LastName' 'Codd,Date') ('FirstName' 'E.F.,C.J.')
┌───────┐
│1      │
└Boolean┘
      T
── SandP.Employee ─────────────────────────────────
 ┌AUTOKEY┐  ┌LastName────────┐  ┌FirstName───────┐
 ↓1      │  ↓Codd            │  ↓E.F.            │
 │2      │  │Date            │  │C.J.            │
 └Int8───┘  └Char(16)────────┘  └Char(16)────────┘
── 2 rows by 3 columns ────────────────────────────
~~~

At any time, using the →[*.MethodList.SetPrimaryKey] method,  another column may be designated as
the primary key, provided the column has no duplicate values:

~~~
     T.SetPrimaryKey 'LastName'
┌───────┐
│0      │
└Boolean┘
      T
── SandP.Employee ──────────────────────
 ┌LastName────────┐  ┌FirstName───────┐
 ↓Codd            │  ↓E.F.            │
 │Date            │  │C.J.            │
 └Char(16)────────┘  └Char(16)────────┘
── 2 rows by 2 columns ─────────────────
~~~

Note that the AUTOKEY column is now invisible, and may not be referenced. When changing the primary
key, FlipDB maintains all foreign key references to the table.

The →[*.MethodList.CreatePrimaryKey] method may be used to create a new column and simultaneously
specify it as the primary key provided the table is newly created, and has no rows.

## Alternate Keys

Alternate Keys provide additional uniqueness constraints. A table may have one or more alternate
keys, and each alternate key may composed of more than 1 column (multi-column keys are called
compound keys). However, no column can belong to more than one alternate key. The
→[*.MethodList.SetAltKey] method specifies alternate keys, and may be used at any time, provided
the specified columns form unique values:

~~~
       J=T.SetPrimaryKey 'AUTOKEY'
       J=T.SetAltKey 'LastName,FirstName'
       T
── SandP.Employee ─────────────────────────────────
 ┌AUTOKEY┐  ┌LastName────────┐  ┌FirstName───────┐
 ↓1      │  ↓Codd            │  ↓E.F.            │
 │2      │  │Date            │  │C.J.            │
 └Int8───┘  └Char(16)────────┘  └Char(16)────────┘
── 2 rows by 3 columns ────────────────────────────
~~~

## Foreign Keys

Foreign keys are columns whose values refer to rows in another table. In FlipDB, foreign keys
always refer to the primary key of the foreign table. The →[*.MethodList.CreateForeignKey] method
creates a new column and specifies it as foreign key. The →[*.MethodList.SetForeignKey] method
will specify that an existing, populated column is now a foreign key, subject to referential
integrity. Note that this is a method of the Column object, not the Table object.

## Table Keys and the Key Property of DataTables

The above covers the concept of keys for Table objects. These keys are part of the schema of a
database, and FlipDB enforces uniqueness and referential integrity for these keys. These keys
should not be confused with the →[*.DataTable.Properties.Key] property of DataTable objects.
This property may represent a primary, alternate, or foreign key of a DataTable, but no
constraints are enforced. The Keys property is a transient specification used only by certain
methods to look up the rows of one DataTable in another DataTable.

