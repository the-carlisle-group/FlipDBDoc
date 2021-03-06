# HTTP Methods and the FlipDB Server Object

The FlipDB Server object provides four methods that implement the uniform interface of HTTP:  Get,
Put, Delete and Post. While these methods are provided to allow access to FlipDB over the
internet, we can exercise them directly in the FlipDB session or in a script. This is not only
useful for pedagogical purposes; being able to manipulate FlipDB resources with the uniform
interface of HTTP in a stand-alone environment or directly on the server is surprisingly useful.
The Get method is used to retrieve a resource:

~~~
      s=Scripting.Server
      d=s.Get '/Databases/SandP'
~~~

Here we have extracted the Suppliers and Parts database. The argument to the Get method is a string
specifying the path of the resource. This is known as a URI, or uniform resource identifier. This
is simply what you would type in the address bar of a Web browser after a domain name like
flipdb.com to view the resource over the web. We can extract the parts table P directly from the
server as well:

~~~
      s.Get '/Databases/SandP/P'
── SandP.P ────────────────────────────────────────────
 ┌PNO────┐  ┌PNAME──┐  ┌COLOR──┐  ┌WEIGHT┐  ┌CITY────┐
 ↓P1     │  ↓Nut    │  ↓Red    │  ↓12    │  ↓London  │
 │P2     │  │Bolt   │  │Green  │  │17    │  │Paris   │
 │P3     │  │Screw  │  │Blue   │  │17    │  │Oslo    │
 │P4     │  │Screw  │  │Red    │  │14    │  │London  │
 │P5     │  │Cam    │  │Blue   │  │12    │  │Paris   │
 │P6     │  │Cog    │  │Red    │  │19    │  │London  │
 └Char(2)┘  └Char(5)┘  └Char(5)┘  └Int8──┘  └Char(10)┘
── 6 rows by 5 columns ────────────────────────────────
~~~

Similarly, the CITY column can be retrieved by specifying its URI:

~~~
          s.Get '/Databases/SandP/P/CITY'
┌────────┐
↓London  │
│Paris   │
│Oslo    │
│London  │
│Paris   │
│London  │
└Char(10)┘
~~~

Note that the Get method allows us to directly address a resource from the server while the object
model requires us to step through the hierarchy one level at time. Using the object model to
access the CITY column requires us to first extract the database using the GetDatabase method:

~~~
      d=s.GetDatabase 'SandP'
~~~

then use the resulting database object to extract the table:

~~~
       t=d.GetTable 'P'
~~~

and finally use the GetColumn method of the table object to extract the column:

~~~
       t.GetColumn 'CITY'
┌────────┐
↓London  │
│Paris   │
│Oslo    │
│London  │
│Paris   │
│London  │
└Char(10)┘
~~~

Every object that is named and saved in FlipDB has a URI and thus may be retrieved using the Get
method. In addition to databases, tables, and columns  we can access items saved under the Objects
node on the server:

~~~
      q=s.Get '/Objects/Queries/Q1'
      q.Execute 0
── Key:SNO ──────────────────────────────────
 ┌SNO────┐  ┌SNAME───┐  ┌STATUS┐  ┌CITY────┐
 ↓S1     │  ↓Smith   │  ↓20    │  ↓London  │
 │S4     │  │Clark   │  │20    │  │London  │
 └Char(2)┘  └Char(10)┘  └Int8──┘  └Char(10)┘
── 2 rows by 4 columns ──────────────────────
~~~

Here we have accessed the query named Q1 that is in the Queries folder.

While the Get method allows us to retrieve a resource, the Put method allows us to create or
replace a resource:

~~~
     t2=s.Put '/Databases/SandP/CopyOfParts' t
     t2
── SandP.CopyOfParts ──────────────────────────────────
 ┌PNO────┐  ┌PNAME──┐  ┌COLOR──┐  ┌WEIGHT┐  ┌CITY────┐
 ↓P1     │  ↓Nut    │  ↓Red    │  ↓12    │  ↓London  │
 │P2     │  │Bolt   │  │Green  │  │17    │  │Paris   │
 │P3     │  │Screw  │  │Blue   │  │17    │  │Oslo    │
 │P4     │  │Screw  │  │Red    │  │14    │  │London  │
 │P5     │  │Cam    │  │Blue   │  │12    │  │Paris   │
 │P6     │  │Cog    │  │Red    │  │19    │  │London  │
 └Char(2)┘  └Char(5)┘  └Char(5)┘  └Int8──┘  └Char(10)┘
── 6 rows by 5 columns ────────────────────────────────
~~~

Here we have created a copy of the parts table in the suppliers and parts database. The Put method
requires two items in its argument: the URI to be created or replaced, and some representation of
the resource to create or replace. In this case we have used the  reference t, which is just
original parts table P. The result of the Put method is a reference to the newly created object.
We can use the Put method to update our query as well. Let's change the Select property and save it:

~~~
      q.Select='SNO,SNAME'
      q=s.Put '/Objects/Queries/Q1' q
~~~

Now we will verify that we actually updated the query by retrieving it again and executing it:

~~~
      (s.Get '/Objects/Queries/Q1').Execute 0
── Key:SNO ────────────
 ┌SNO────┐  ┌SNAME───┐
 ↓S1     │  ↓Smith   │
 │S4     │  │Clark   │
 └Char(2)┘  └Char(10)┘
── 2 rows by 2 columns
~~~

The Delete method does what it says and deletes a resource:

~~~
      s.Delete '/Databases/SandP/CopyOfParts'
┌───────┐
│0      │
└Boolean┘
~~~

