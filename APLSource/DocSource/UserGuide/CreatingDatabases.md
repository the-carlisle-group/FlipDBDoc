# Creating Databases, Tables and Columns over HTTP

To create a new, empty database on the server, we use the PUT method:

~~~
PUT /Databases/SandP2 HTTP/1.1
Accept: application/json
Content-Type: application/json
Connection: close
Host: localhost
Content-Length: 0

~~~

~~~
HTTP/1.1 201 Created
Date: Fri, 19 Jun 2015 13:50:26 GMT
Server: Rumba
Connection: close
Content-Length: 0
Content-Type: text/html

~~~

~~~
 **** Example goes here *****
~~~

We can then add an empty table to this database:

~~~
 **** Example goes here *****
~~~

And finally add a column to this new table:

~~~
 **** Example goes here *****
~~~

All of these examples above use the minimum possible representation of the resource in order to
create it. This can be useful for small, incremental changes to, say, an existing table, but
requires too many round trips to the server when establishing a complete database structure. To
create a complete database, we can provide the complete structure in the resource definition:

~~~
 **** Example goes here *****
~~~

Note further that for Database, Tables, and Columns, PUT will not replace an existing resource. If
the resource already exists, you must first explicitly delete before using PUT.

