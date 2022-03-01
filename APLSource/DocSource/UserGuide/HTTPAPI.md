# The HTTP API

## The API and Web Browsers

Web browsers only support two HTTP methods: GET and POST. It is not possible to execute DELETE and
PUT directly from the browser. While other user agents can do DELETEs and PUTs, web browsers have
limited functionality by design. In order to get around this limitation, FlipDB allows methods
from its object model to be accessed as resources. For example, to create a new table in the SandP
database we can enter this URL in the browser address bar:

~~~
      /Databases/SandP.CreateTable
~~~

Similarly, to delete a column from the parts table of the SandP database:

~~~
      /Databases/SandP/P.DeleteColumn
~~~

The HTML representation, (the page returned to and displayed by the browser) of the method resource
is a form, with fields for entering the method's argument, and an Execute button for posting the
form back to the server. This executes the method on the server on the appropriate object with the
specified argument.

In addition to creating and deleting resources, other FlipDB methods may be used to modify
resources. For example,  the data type of a column may be changed:

~~~
      /Databases/SandP/WEIGHT.ChangeType
~~~

Server methods like CreateDatabase and CreateUser are accessed at the corresponding node. For example:

~~~
     /Databases.CreateDatabase
~~~

and:

~~~
     /Users.CreateUser
~~~

We should probably make Databases and Users actual singleton classes, so that we can have HTML
reps, and put the server methods in there, (Users.Add rather than Server.AddUsers) This would make
the resource hierarchy consistent across the object model and the web. Note that we already have
(almost) a Folder class that is returned from '/Objects'
