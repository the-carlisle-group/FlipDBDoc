# Introduction

FlipDB is a relational database management system (RDBMS), programming language, and object model.

FlipDB Desktop is designed from the ground up with a focus on solving the complex data problems of the
mortgage and asset finance business, but it is a general purpose application suitable for a wide
range of industries and problems.

FlipDB Desktop provides immediate, out-of-the-box functionality for ad
hoc data analysis, manipulation and reporting, while simultaneously providing a foundation for
building enterprise-wide, multi-user, industrial-strength solutions.

Mortgages push the limits of many systems due to the quantity of data items, variety of products,
constant innovation, and a never-ending stream of file formats and standards. Mortgage industry
reporting requires repeated full-table scans, taxing the limits of traditional database management
systems. Whole-loan mortgage trading and securitization requires massive amounts of data analysis,
manipulation and reporting, often over an extended period of time. Data conversion is a major
problem in the mortgage industry, and ETL (extract, translate, and load) processes are the daily
routine of mortgage trading, rather than periodic one-off tasks for an IT department. Many facets
of the mortgage industry require just the right balance of transaction-oriented and analytic
database features as well spreadsheet functionality, not often found in a single product.
Conventional solutions to this problem require multiple products, a large technology stack, and a
wide range of technical skills and staff. The results are often difficult achieve, costly to build
and maintain, and error prone. FlipDB provides a single, coherent and unified environment ideally
suited for building solutions to a wide range of mortgage related problems.

## The FlipDB GUI

FlipDB has a rich, native Windows GUI, which is a requirement for complex, data intensive
applications. A browser-based application simply cannot provide a satisfactory user experience for
the type of work that FlipDB is designed to do. The major components of the GUI include:

|-|-|
|Server Explorer|For viewing, creating, and modifying the contents of a FlipDB server which includes users, databases, tables, reports, queries, scripts, and other ancillary objects.|
|Table Explorer|For viewing, querying, analyzing and manipulating a single table.|
|Query Results Viewer|For displaying the results of a query, with drill-down capabilities.|
|Import Wizard|An extensive wizard with step-by-step instructions and feedback for importing data into FlipDB.|
|Report Writer|For creating, running and viewing reports.|
|Expression Builder|For entering and testing a single column-returning expression, with context sensitive help and auto-complete for functions and column names.|
|Table Editor|For editing the contents of a table or the result of a query.|
|Script Editor|For creating, editing and running FlipDB scripts.|
|Session|For entering FlipDB expressions for immediate execution and feedback, and for debugging scripts.|
|Tracer|For stepping line-by-line through queries, reports, and scripts.|

## The RDBMS

At the heart of FlipDB is a column-oriented, multi-user, relational database management system. The
DBMS is specifically designed to support a mix of on-line transaction processing (OLTP) and
on-line analytic processing (OLAP) in a unified environment with no duplication of data.

FlipDB uses extreme multi-value concurrency control (XMVCC), where no data is ever overwritten. In
addition to being the foundation for an extremely reliable and robust data store for concurrent
use, XMVCC makes FlipDB a temporal DBMS in that a query may not only be applied to the current
state of a database, but to any past state as well. The practical uses of this feature cannot be
overstated. Finally, XMVCC ensures complete and secure audit trails, with full history of every
change to every data item. FlipDB supports atomicity, consistency, isolation, and durability (ACID).

## The Object Model and Programming Language

All of the functionality of FlipDB, including the RDBMS, ancillary data structures, and the report
writing tools, is exposed through a single, unified object model. The object model is simple yet
provides a rich set of functionality to solve a wide range of problems.

FlipDB has a full-featured programming language specifically designed to work with column-oriented
databases, and fully integrated with the FlipDB object model. This means that the FlipDB database
query language and programming language are one and the same.

## Installation and Usage Scenarios.

There are three ways to install and use FlipDB. In a single-user scenario, FlipDB is installed in
standalone mode on a single machine for a single user. In this scenario, the data folder (where
databases, objects, etc., are stored) is usually on the same machine.

In a server-less workgroup scenario, FlipDB is installed in standalone mode for each user on their
machine as in a single-user scenario, but the data folder for each user is on a common network
drive, accessible to all users. This allows users to work with each other's data, and is analogous
to having multiple users of Microsoft Excel sharing workbooks on network drive.

In a web server scenario, FlipDB is installed on a server machine in server mode, where it listens
and responds to HTTP requests.  Users can access FlipDB's built-in web application by simply
pointing their web browser to the appropriate address. In addition, other internal or external
applications can access the FlipDB HTTP API, sending and receiving JSON or XML.

