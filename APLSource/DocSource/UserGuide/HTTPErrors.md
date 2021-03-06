# HTTP Errors

In case of an error, the FlipDB server will return one of the following HTTP status codes: 400 (Bad
Request), 404 (Not Found), 409 (Conflict), 411 (Length Required), 413 (Payload too Large), or 500
(Internal Server Error)

In addition to reporting the HTTP status code and reason phrase in the response line (per the HTTP
specification), FlipDB returns an error object in the payload. The error object contains the HTTP
status code and reason phrase (duplicated from the response line), as well as a FlipDB error
number and message. The FlipDB message will usually contain helpful information for tracking down
the problem.

For example, in attempting to GET a column from a non-existing database, the obvious HTTP 404 error
is returned, indicating the resource is not found, but in addition, the FlipDB error message in
the payload tells us specifically that the database itself, not just the column, is not found:

~~~
GET /Databases/NoSuchDB/Table1/Column1 HTTP/1.1
Accept: application/json
Connection: close
Host: localhost

~~~

~~~
HTTP/1.1 404 Not Found
Content-Type: application/json
Date: Tue, 09 Jun 2015 21:38:13 GMT
Server: Rumba
Connection: close
Content-Length: 134
{
Class: "Error",
ErrorMessage: "Database not found. (NoSuchDB)",
ErrorNumber: 702,
ReasonPhrase: "Not Found",
StatusCode: 404
}
~~~

Attempting to delete a column, table, or row that would cause a referential integrity violation
yields a 409 (Conflict) error:

~~~
DELETE /Databases/SandP/P HTTP/1.1
Connection: close
Host: localhost
Accept: */*

~~~

~~~
HTTP/1.1 409 Conflict
Content-Type: application/json
Date: Thu, 11 Jun 2015 10:20:55 GMT
Server: Rumba
Connection: close
Content-Length: 135
{
Class: "Error",
ErrorMessage: "Referential integrity violation.",
ErrorNumber: 716,
ReasonPhrase: "Conflict",
StatusCode: 409
}
~~~

An invalid query can return either a 404 a 400 error, depending on the reason for failure. For example:

~~~
GET /Databases/SandP/P?WhereQuota=ten HTTP/1.1
Accept: application/json
Connection: close
Host: localhost

~~~

~~~
HTTP/1.1 400 Bad Request
Content-Type: application/json
Date: Thu, 11 Jun 2015 10:34:32 GMT
Server: Rumba
Connection: close
Content-Length: 159
{
Class: "Error",
ErrorMessage: "Invalid property value. (Property must be an integer)",
ErrorNumber: 749,
ReasonPhrase: "Bad Request",
StatusCode: 400
 }
~~~

If the query requires specific resources in order to execute, like additional tables to link to or
a specific column, and these are not found, then a 404 may result:

~~~
GET /Databases/SandP/P?Select=PNO,NoSuchCol HTTP/1.1
Accept: application/json
Connection: close
Host: localhost

~~~

~~~
HTTP/1.1 404 Not Found
Content-Type: application/json
Date: Thu, 11 Jun 2015 10:39:50 GMT
Server: Rumba
Connection: close
Content-Length: 133
{
Class: "Error",
ErrorMessage: "Column not found. (NoSuchCol)",
ErrorNumber: 704,
ReasonPhrase: "Not Found",
StatusCode: 404
}
~~~

