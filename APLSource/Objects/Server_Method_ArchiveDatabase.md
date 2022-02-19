# ArchiveDatabase Method

Applies to:{.prefix}

→[##.##.Server]{.info}

This method archives a database.

The argument is composed of 1 item:

|-|-|
|1|Database name|String|

The result is composed of 1 item:

|-|-|
|1|Return code|0|

The ArchiveDatabase method archives a database by first making a full, backup copy of the
existing database and then removing all inactive rows from the existing database. FlipDB employs
an automatic archive naming and numbering system.

For example:

~~~
      S.ArchiveDatabase 'SANDP'
~~~

will create a new database SANDP_ARCH_001, and remove all inactive rows from SANDP.

