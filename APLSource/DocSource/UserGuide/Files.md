# File Storage

FlipDB provides file storage and retrieval.
Files may be associated with the server in general
and stored under the URL:

~~~
/Files
~~~

Files may also be stored in association with a particular database.
For example, files associated with the SandP database are stored under the URL:

~~~
/Databases/SandP/Files
~~~

Files may be stored in an arbitrary heirarchy of sub folders. For example, a
PUT to:

~~~
/Databases/SandP/Files/MyFolder1/Myfolder2/MyFile.txt.
~~~

will associate the payload file with this folder heirarchy. Note
that folders only implicitly exist on the server to the extent that
they have a file in them.

## Putting Files on the Server

The PUT method will create or replace a single file on the server.
The URL for a PUT is the desired full path and name of the file.
For example:

~~~
PUT /Files/MyFolder/Hello World.Text

blah blah

~~~

## Posting Files to the Server

The POST method will create or replace multiple files on the server.
The URL for a POST is the parent folder that will contain all of the files in the payload.
The payload of a POST may be one or more files. If it is one file, and that file is a .zip
file, then the server uncompresses it and creates or replaces a resource for each file in
the compressed folder. To put a zipped folder on the server as a single binary file,
that is without unzipping it, the PUT method should be used.


## Getting Files

The GET method is used to retrieve files and folders.
A GET on a file resource returns the bytes of the file as the payload.
The Content-Type is always application/octet-stream, regardless of
the Accept header in the request.

~~~
GET /Files/Files/.
...
...
~~~

A GET on a folder resource returns either a list of the contents of the folder
including files and sub folders, or all of the files in the folder in a
single zip file, depending on the Accept request header.

If the accept request header is application/json or text/html,
then the result is a list of the contents of the folder:

~~~
GET /File/folder....
...
...
~~~

If the Accept request header is application/zip, then
the result is a compressed folder containing the actual files.

~~~

~~~

Analternatvie to using the accept header, to support web browsers,

~~~
GET /File/folder?type=zip
...
...
~~~



~~~
GET /File/Subfolder

~~~

## Deleting Files

The DELETE method is used to delete a file or a folder and all of its contents.

~~~
DELETE /Files/File
...
...
~~~



