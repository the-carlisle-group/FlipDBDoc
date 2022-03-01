# ImportTable Method

Applies to:{.prefix}

→[##.##.Database]{.info}

The argument is composed of 2 items:

|-|-|
|1|TableName|String|
|2|Folder|String|

The result is composed of 1 item:

|-|-|
|1|Table|Table|

This method creates and populates a new table by importing the contents of a properly prepared
folder. (To import or update a table from a CSV file, Excel file, or an ODBC data source, use the
Import object)

A properly prepared folder contains a set of files, one file for each column of the table.  Each
file is composed of a 4096 byte header, followed by col data. The 4096 byte header is composed as follows:

|Item|Bytes|Total Bytes|
|-|-|-|
|Array file identifier; 32 bit int  (19221027)|Array file version; 32 bit int  (1)|QuadDR data type; 32 bit int|Column width; in bytes; 32 bit int|Filler; 28 zeros as 32 bit ints|Filler|Column Type (Left justified, padded with blanks)|Column Name (Left justified, padded with blanks)|Filler|
|4|4|4|4|112|128|32|128|3680|
|4|8|12|16|128|256|288|416|4096|

The column type must be a valid FlipDB data type, and correspond exactly with the QuadDR data type
and column width. Acceptable values may be drawn from the following table:

|FlipDB Data Type|QuadDR|Width in Bytes|
|-|-|-|
|Boolean|Char(x)|CharW(x)|CharXW(x)|Int8|Int16|Int32|Float|Decimal(x)|Date|Time|DateTime|
|83|80|160|320|83|163|323|645|323|323|323|645|
|1|x|2x|4x|1|2|4|8|4|4|4|8|

The column name may be any string. FlipDB will convert the string to a valid FlipDB name, and
ensure uniqueness. If the column names are left blank, FlipDB will name them Column1, Column2 … ColumnN.

Following the header, each file contains the data for the column. Each column must contain the same
number of data items, and each data item must be the proper width. This implies a specific file
size. For example a file with 100 Ints32’s will be 4096 + 400 = 4496 bytes.

Each file in the folder must be in the above format. Any folder that contains any extraneous files
or files that are not formatted properly will not import, and signal an exception.

