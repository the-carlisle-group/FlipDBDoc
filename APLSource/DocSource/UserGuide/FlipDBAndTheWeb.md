# FlipDB and the Web

FlipDB is a web server. If an installation is launched in Server mode, FlipDB is listening for HTTP
requests on the standard HTTP port 80. You can point your web browser to the appropriate IP
address or domain name, and begin browsing the FlipDB server. This built-in web application allows
you to navigate through all of the content of a FlipDB server, execute queries, and create, edit,
and delete objects.

FlipDB can respond to HTTP requests with various content types depending on the preferences of the
client. For web browsers, this is usually an HTML document. FlipDB can also respond with JSON.
If you are writing a JavaScript based web application that needs to
communicate with a FlipDB server, you might ask for and receive JSON. The details of communicating
with a FlipDB server via HTTP are detailed in the section HTTP API.

A web resource is anything that can be identified, retrieved or manipulated on the Web. FlipDB
objects map to web resources in an obvious and intuitive way. A database, a table, and a column
are all resources. A query object is a resource, and the folder in which the query object is saved
is a resource. In addition, the result of a query object (a DataTable) is also a resource. Every
resource has an address or URL (uniform resource locater). For example, a list of the databases
on the server may be found at:

~~~
      /Databases
~~~

And the suppliers and parts database, if it exists, may be found at:

~~~
      /Databases/Parts
~~~

While the part table may be found at:

~~~
      /Databases/Parts/P
~~~

Each representation of a resource provides information about and links to the resources below it in
the hierarchy. Thus the /Databases page provides links to all the databases, the /Database/SandP
page provides links to all the tables in the database, and the the /Databases/SandP/P page provides
links to all the columns in the parts table. In addition to /Databases, other top level entry
points include:

~~~
      /Objects
~~~

which provides access to folders, reports, scripts and other objects, and:

~~~
      /Files
~~~

Which provides general file storage and retrieval, and:

~~~
      /Documentation
~~~

which provides access to documentation on the object model and functions, and:

~~~
      /Users
~~~

which provides access to users, user groups and permissions, and:

~~~
      /Log
~~~

which provides access to log files.

These, then, are resources. HTTP operates on resources via a limited set of methods or verbs. These
include GET, PUT, DELETE and POST. The GET method is used to retrieve resources, the PUT method to
create or replace resources, the DELETE method to delete resources, and the POST method for
modifying resources and anything else that does not fit neatly into  GET, PUT, or DELETE. Not all
methods apply to all resources. For example, you may not DELETE, PUT or POST to documentation
resources. Of course, all access is dependent on the user's level of authorization.

While the FlipDB API supports all of these methods, Web browsers generally only support two HTTP
methods: GET and POST. It is not possible to execute DELETE and PUT directly from the browser.
While other user agents can do DELETEs and PUTs, web browsers have limited functionality by
design. In order to get around this limitation and to provide full functionality from a Web
browser, FlipDB allows methods from its object model to be accessed as resources. For example, to
create a new table in the SandP database we can enter this URL in the browser address bar:

~~~
      /Databases/SandP.CreateTable
~~~

Similarly, to delete a column from the parts table of the SandP database:

~~~
      /Databases/SandP/P.DeleteColumn
~~~

The HTML representation, (the page returned to and displayed by the browser) of the method resource
is a form, with fields for entering the method's argument, and an Execute button for posting the
form back to the server. This executes the method on the appropriate object with the argument
specified in the form.

In addition to creating and deleting resources, other FlipDB methods may be used to modify
resources. For example,  the data type of a column may be changed:

~~~
      /Databases/SandP/WEIGHT.ChangeType
~~~

Server methods like CreateDatabase and CreateUser are accessed under /Databases and /Users
respectively, even though they are members of the Server object. For example:

~~~
     /Databases.CreateDatabase
~~~

and:

~~~
     /Users.CreateUser
~~~

Links to all of these method resources are provided in the corresponding representation of the
associated object for easy discovery.

Table resources may be queried to return the value for the table rather than the schema of the table.

