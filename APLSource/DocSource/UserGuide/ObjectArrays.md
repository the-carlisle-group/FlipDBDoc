# Object Arrays

FlipDB supports arrays of objects. An object array is an array of zero or more objects. Object
arrays form naturally in FlipDB. For example, the following is a three item object array of DataColumns:

~~~
      A=(1 2 3) ('Codd,Date') 4.5
      A
 ┌────┐  ┌───────┐  ┌──────┐
 ↓1   │  ↓Codd   │  │4.5   │
 │2   │  │Date   │  └Dec(1)┘
 │3   │  └Char(4)┘
 └Int8┘
~~~

Many structural functions like →[##.##.Reference.Functions.shape],
→[##.##.Reference.Functions.take] and →[##.##.Reference.Functions.replicate] work on object arrays:

~~~
      shape A
┌────┐
│3   │
└Int8┘
      2 take A
 ┌────┐  ┌───────┐
 ↓1   │  ↓Codd   │
 │2   │  │Date   │
 │3   │  └Char(4)┘
 └Int8┘
      1 0 1 replicate A
 ┌────┐  ┌──────┐
 ↓1   │  │4.5   │
 │2   │  └Dec(1)┘
 │3   │
 └Int8┘
~~~

To create an empty object array, use the →[*.emptyArray] function:

~~~
      E=emptyArray
      E
[Empty object array]
~~~

Object arrays may contain objects of different types. For example, the following is a two-item
object array containing a DataColumn and a →[*.DataTable]:

~~~
      A=(1 2 3) (('Name' 'Beethoven') ('DOB' 'December 17'))
      A
 ┌────┐  ────────────────────────────
 ↓1   │   ┌Name─────┐  ┌DOB────────┐
 │2   │   │Beethoven│  │December 16│
 │3   │   └Char(9)──┘  └Char(11)───┘
 └Int8┘  ── 0 rows by 2 columns ─────
~~~

Many properties of objects return an object array. For example, the →[*.Tables] property of the
→[*.Objects.Database] object returns an array of Table objects. If all of the objects in an
array are the same type, then methods and properties of that type may be called on the object
array, effectively calling the method or property on each of the component objects:

~~~
      D=Scripting.Server.OpenDatabase 'SandP'
      D.Tables.Name
 ┌───────┐  ┌───────┐  ┌───────┐
 │S      │  │P      │  │SP     │
 └Char(1)┘  └Char(1)┘  └Char(2)┘
~~~

Note that while the →[*.Table.Properties.Name] property applied to an array of tables returns an
array of scalar Char columns, the →[*.TableNames] property of a database returns a single,
simple Char column:

~~~
      D.TableNames
┌───────┐
↓S      │
│P      │
│SP     │
└Char(2)┘
~~~

