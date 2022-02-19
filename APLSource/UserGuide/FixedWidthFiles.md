# Fixed Width Files

Fixed width files are ASCII files where each field occupies the same set of bytes in each record.
(There are also fixed width EBCDIC files. These are discussed in detail in the section on COBOL
files.) Importing a fixed width file requires the name, starting byte and the width in bytes of
each field you wish to import. All fields are initially imported as Char columns and  must be
converted to the appropriate FlipDB data type by applying a casting function like toInteger or
toDate. In some cases, a fixed width file will not have embedded new line characters, and the
record length in bytes will have to be explicitly specified using the RecordLength property.

