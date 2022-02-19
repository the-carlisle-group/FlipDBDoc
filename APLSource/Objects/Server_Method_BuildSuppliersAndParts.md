# BuildSuppliersAndParts Method

Applies to:{.prefix}

→[##.##.Server]{.info}

This method builds the longstanding Supplier and Parts database from C.J. Date’s writings.

The argument is composed of one item:

|-|-|
|1|Void|Void|

The result is composed of one item:

|-|-|
|1|SandP Database|Database object |

The suppliers and parts database contains 3 tables: S for Suppliers, P for Parts, and a cross
reference table SP that shows what parts supplied by which suppliers.

|SNO|SNAME|STATUS|CITY|
|-|-|-|-|
|S1|Smith|20|London|
|S2|Jones|10|Paris|
|S3|Blake|30|Paris|
|S4|Clark|20|London|
|S5|Adams|30|Athens|

|SNO|PNO|QTY|
|-|-|-|
|S1|P1|300|
|S1|P2|200|
|S1|P3|400|
|S1|P4|200|
|S1|P5|100|
|S1|P6|100|
|S2|P1|300|
|S2|P2|400|
|S3|P2|200|
|S4|P2|200|
|S4|P4|300|
|S4|P5|400|

|PNO|PNAME|COLOR|WEIGHT|CITY|
|-|-|-|-|-|
|P1|Nut|Red|12.0|London|
|P2|Bolt|Green|17.0|Paris|
|P3|Screw|Blue|17.0|Oslo|
|P4|Screw|Red|14.0|London|
|P5|Cam|Blue|12.0|Paris|
|P6|Cog|Red|19.0|London|
