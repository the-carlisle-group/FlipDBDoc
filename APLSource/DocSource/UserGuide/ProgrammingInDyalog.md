# Programming FlipDB in Dyalog APL

Programming FlipDB in Dyalog APL is distinguished from
programming FlipDB in FlipDB Desktop. FlipDB Desktop has its
own session, and does not use APL characters.

Aside from this section, written expressly for the APL programmer,
all of the examples in the
user guide and reference manual are shown in the FlipDB
Session. These translate directly for the APL programmer
by simply replacing = with ←, and double quotes with single quotes.
In addition, the FlipDB Session displays results verbosely.
The APL programmer can access this by running the Display method
on any object.

## Installation

FlipDB is is distributed in a folder named FlipDB, containing
a workspace named FlipDB.dws, and other various support files.
Uncompress and place the folder anywhere. Load the workspace,
and you are ready to begin.

## Getting Started

To do anything in FlipDB, you need to create a server.
A server may be thought of as a project, and indeed the
terms are interchangable. (FlipDB Desktop uses the term project,
as this indicates that the server is opened exclusively for
direct access rather than via HTTP over the internet.)

A server is represented by a folder on disk.
To create a new server, use the CreateServer function, located in
the #.FlipDB.Admin namespace:

~~~
      s←CreateServer 'c:FlipDBMyProject'
~~~

When creating a server, the folder must not already exist,
but the parent folder must exist. To open an existing server,
use the OpenServer function:

~~~
      s←OpenServer 'c:FlipDBMyProject'
~~~

Both of these functions return a server object.
With the server object in hand, you are ready to
create databases, import data, and run queries.

## Creating a Database

Create a new database using the Server's createDatabase
method:

~~~
      d←s.CreateDatabase 'MyDB'
~~~

FlipDB has two sample databases built in.
You can create the famous suppliers and parts
database of C.J. Date using:

~~~
      d←s.BuildSuppliersAndParts 'SandP'
~~~

Or you can create the less well known
sample mortgage loan database:

~~~
      d←s.BuildLoanSample 'SampleLoan'
~~~

To access an existing database,
use the server's GetDatabase method:

~~~
      d←s.GetDatabase 'SandP'
~~~

or the Get method, which accesses an item on the server
via its URL:

~~~
      d←s.Get '/Databases/SandP'
~~~

## Importing a CSV File

To import a CSV file, create an import object,
set a few properties, and run the Execute method:

~~~
      i←s.NewImport ''
      i.SourceName←'C:ile.txt'
      i.SourceType←'CSV'
      i.TargetDatabaseName←'MyDB'
      i.TargetTableName←'MyTable'
      t←i.Execute 0
~~~
The result of Execute is a table object.

## Executing a Query

To execute a query, create a query object,
set a few properties, and run the Execute method:

~~~
      q←s.NewQuery ''
      q.DatabaseName←'MyDB'
      q.TableName←'MyTable'
      q.Where←'Rate > 10'
      dt←q.Execute 0
~~~

The result of the Execute method is a datatable object.

## Saving Objects on the Server

By definintion, databases and tables are stored in
the server. Other flipdb object types, like Import, Query,
and DataTable, may be stored as well using server's Put method:'

~~~
      s.Put '/Objects/MyFirstQuery' q
      s.Put '/Objects/MyFirstImport' i
      s.Put '/Objects/SomeData' dt
~~~

These may be retrieved using the Get method:

~~~
      q←s.Get '/Objects/MyFirstQuery'
      i←s.Get '/Objects/MyFirstImport'
      dt←s.Get '/Objects/SomeData'
~~~

Any item may be deleted using the Delete method:

~~~
      s.Delete '/Objects/SomeData'
~~~





