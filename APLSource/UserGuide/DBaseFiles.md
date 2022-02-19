# dBase Files

dBase files are the native file type of dBase, FoxPro, and other PC-based database applications.
They usually have a "dbf" extension. They contain data on a single table (unlike, say, a Microsoft
Access .mdb file which contains a set of tables.)

A dBase file may have character, date, and numeric fields. FlipDB will automatically convert these
to the corresponding FlipDB data types. dBase files may contain deleted records. These records are
not imported.

dBase files are well-structured and they are extremely fast to import. If receiving data from a
dBase system, always request native dBase files rather than CSV or Excel files.

