# ConnectionString Property

Applies to:{.prefix}

→[##.##.Import]{.info}

This property specifies the connection string for SourceTypes ADO and ODBC. It is a string.

Note that for SourceType ADO the →[Provider] property and the →[SourceName] property also
contribute to the connection string, defining the "Provider" and  "Data Source" connection
string parameters respectively. These two parameters may either be provided by specifying these
properties or by specifying them directly in the ConnectionString property.

Similarly for SourceType ODBC, the →[SourceName] property contributes to the connection string by
providing the DSN (Data Source Name) parameter. The Provider property is not used with ODBC

