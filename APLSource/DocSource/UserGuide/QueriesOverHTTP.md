# Queries over HTTP

The easiest way to query a FlipDB database is to simply add an empty query string to the end of a
table URI. This tells the server to return the value of the table rather than the schema of the table:

~~~
GET /Databases/SandP/P? HTTP/1.1
Accept: application/json
Connection: close
Host: localhost

~~~

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Date: Fri, 12 Jun 2015 13:11:16 GMT
Server: Rumba
Connection: close
Content-Length: 1368
 {
    Class: "DataTable",
    Columns: [
             {
                 Class: "Char",
                 Name: "PNO",
                 Type: "Char(2)",
                 Scale: 0,
                 Structure: 1,
                 Value: ["P1", "P2", "P3", "P4", "P5", "P6"]
             },
             {
                 Class: "Char",
                 Name: "PNAME",
                 Type: "Char(5)",
                 Scale: 0,
                 Structure: 1,
                 Value: ["Nut", "Bolt", "Screw", "Screw", "Cam", "Cog"]
             },
...Truncated
~~~

For simple queries we can add name and value pairs to the query string that correspond to
properties of the FlipDB Query object. For example, we might select only the part number and color
columns, as well as only the first 3 rows:

~~~
GET /Databases/SandP/P?Select=PNO,COLOR&WhereQuota=3 HTTP/1.1
Accept: application/json
Connection: close
Host: localhost

~~~

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Date: Fri, 12 Jun 2015 13:19:40 GMT
Server: Rumba
Connection: close
Content-Length: 540
{
    Class: "DataTable",
    Columns: [
             {
                 Class: "Char",
                 Name: "PNO",
                 Type: "Char(2)",
                 Scale: 0,
                 Structure: 1,
                 Value: ["P1", "P2", "P3"]
             },
             {
                 Class: "Char",
                 Name: "COLOR",
                 Type: "Char(5)",
                 Scale: 0,
                 Structure: 1,
                 Value: ["Red", "Green", "Blue"]
             }
             ]
}
~~~

This technique is fine for simple queries. It also has the advantage of being cacheable. But trying
to place all of the properties of a complex query in the query string of the URL can be difficult
at best. The solution is to construct a query object and send it in the payload of a POST to the
/Execute URI:

~~~
POST /Execute HTTP/1.1
Accept: application/json
Content-Type: application/json
Connection: close
Host: localhost
Content-Length: 80
{
Class: "Query",
DatabaseName: "SandP",
GroupBy: "COLOR",
TableName: "P"
}
~~~

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Date: Fri, 12 Jun 2015 21:48:33 GMT
Server: Rumba
Connection: close
Content-Length: 543
{
    Class: "DataTable",
    Columns: [
             {
                 Class: "Char",
                 Name: "GroupByCOLOR",
                 Type: "Char(5)",
                 Scale: 0,
                 Structure: 1,
                 Value: ["Red", "Green", "Blue"]
             },
             {
                 Class: "Integer",
                 Name: "RowCount",
                 Type: "Int8",
                 Scale: 0,
                 Structure: 1,
                 Value: [3, 1, 2]
             }
             ]
}
~~~

While simplifying the URI, POSTing has the disadvantage of not being cacheable.  We can overcome
this limitation by using a two-step process:  first PUTting the query object to the server as a
resource in its own right, and then GETting this resource. Let's take the above query object
and PUT it to the server:

~~~
PUT /Objects/MyQuery HTTP/1.1
Accept: application/json
Content-Type: application/json
Connection: close
Host: localhost
Content-Length: 80
{
Class: "Query",
DatabaseName: "SandP",
GroupBy: "COLOR",
TableName: "P"
}
~~~

~~~
HTTP/1.1 201 Created
Date: Fri, 12 Jun 2015 21:59:50 GMT
Server: Rumba
Connection: close
Content-Length: 0
Content-Type: text/html

~~~

Now we can execute the query by doing a GET on this new URI with a ? tacked on the end to force an evaluation:

~~~
GET /Objects/MyQuery? HTTP/1.1
Accept: application/json
Connection: close
Host: localhost

~~~

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Date: Fri, 12 Jun 2015 22:26:02 GMT
Server: Rumba
Connection: close
Content-Length: 543
{
    Class: "DataTable",
    Columns: [
             {
                 Class: "Char",
                 Name: "GroupByCOLOR",
                 Type: "Char(5)",
                 Scale: 0,
                 Structure: 1,
                 Value: ["Red", "Green", "Blue"]
             },
             {
                 Class: "Integer",
                 Name: "RowCount",
                 Type: "Int8",
                 Scale: 0,
                 Structure: 1,
                 Value: [3, 1, 2]
             }
             ]
}
~~~

We can also execute this query with slightly different parameters by passing them in the query
string of the URI:

~~~
GET /Objects/MyQuery?GroupBy=CITY HTTP/1.1
Accept: application/json
Connection: close
Host: localhost

~~~

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Date: Fri, 12 Jun 2015 22:31:09 GMT
Server: Rumba
Connection: close
Content-Length: 546
{
    Class: "DataTable",
    Columns: [
             {
                 Class: "Char",
                 Name: "GroupByCITY",
                 Type: "Char(10)",
                 Scale: 0,
                 Structure: 1,
                 Value: ["London", "Paris", "Oslo"]
             },
             {
                 Class: "Integer",
                 Name: "RowCount",
                 Type: "Int8",
                 Scale: 0,
                 Structure: 1,
                 Value: [3, 2, 1]
             }
             ]
}
~~~

Note that this does not change the definition of the saved query, which can only be done with a
PUT. And finally, of course, we can retrieve the definition of the query itself:

~~~
GET /Objects/MyQuery HTTP/1.1
Accept: application/json
Connection: close
Host: localhost
~~~

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Date: Fri, 12 Jun 2015 22:42:39 GMT
Server: Rumba
Connection: close
Content-Length: 9770
{
    Class: "Query",
    AllowOverlappingGroups:{
                               Class: "Boolean",
                               Type: "Boolean",
                               Scale: 0,
                               Structure: 0,
                               Value: 0
                           },
    AsOf:{
             Class: "Integer",
             Type: "Int8",
             Scale: 0,
             Structure: 0,
             Value: 2
         },
...Truncated
~~~

