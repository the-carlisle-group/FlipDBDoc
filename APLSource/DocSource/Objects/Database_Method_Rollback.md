# Rollback Method

Applies to:{.prefix}

→[##.##.Database]{.info}

This method rolls the database back to an earlier state.

The argument is composed of 1 item:

|-|-|
|1|Directive|Various|

The argument may be an integer, date, or datetime. If the argument is a negative integer N (or
zero), the database is rolled back N transactions. If the argument is a positive integer P, then
the database is rolled back to transaction id (TWID) P. If the argument is a date or datetime D,
then the database is rolled back to the last transaction on or before D.

Note that only updates, inserts and deletes to tables are rolled back. Changes to the schema, like
changing a column type, deleting a table, or specifying a foreign key, are not rolled back. In
addition, changes to computed column definitions, reports, etc., are not rolled back.

Note further that in a multi-user scenario, the transaction ids (TWIDs) are not necessarily written
to the transaction table in numerical order, as they would be in a stand-alone situation. This is
because TWIDs are assigned at the beginning of a transaction, but are not written to the
transaction table itself until the end of the transaction. Therefore, when rolling back to a
specific transaction id, it is possible that one or more transactions with numerically lower TWIDs
will be removed as well. For example rolling back to transaction number 15 may remove transaction
14 if 14 completed execution after 15, even though 14 started executing before 15.

Finally, if rolling back changes due to importing to an existing table, note that the import
process uses one transaction for updating existing rows, and another transaction for adding new
rows. Thus, depending on the situation, rolling back the results of an import may require an
argument of -1 or -2.

