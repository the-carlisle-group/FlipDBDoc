# CSV Files

CSV files are text files where each field is separated  by a delimiter, usually a comma. (Thus the
abbreviation CSV for Comma Separated Values.) CSV files may or may not have field names on the
first row. The HasHeader property indicates whether or not the first row contains field names. CSV
files may have any character as the delimiter, not just a comma. Popular delimiters are tabs,
spaces, and pipes (shift M on your keyboard). The delimiter is specified using the Delimiter property.

To handle the case where the delimiter character is embedded inside a field CSV files have a "text
qualifier" character that surrounds the value of the field. This is usually the double quote
character. The text qualifier is specified using the TextQualifier property.

All fields are initially imported as Char columns and must be converted to the appropriate FlipDB
data type by applying a casting function like toInteger or toDate.

