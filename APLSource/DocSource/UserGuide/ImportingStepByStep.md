# The Import Wizard Step by Step

The Import Wizard is a graphical, step-by-step way to create, modify and execute an Import object.
The wizard is divided into 6 pages or steps.

### Step 1: Specify the Data Source Type

The first wizard page specifies the SourceType property of the associated Import object. This may
be "CSV", "Excel", "dBase", "ADO", "ODBC", "FixedWidth" and "FlipDB". This is the only piece of
information required on this page.

### Step 2: Specify SourceType Specific Properties.

Once the SourceType has been specified, the Wizard knows what other SourceType-specific properties
of the Import object are required. These are all requested on the second page. For example, if the
SourceType is dBase, the only item required is the SourceName, which is the dBase file name.
However, if the SourceType is FixedWidth, then the required items include SourceName,
FixedWidthLayout and RecordLength. This page is the only wizard page that depends on the
SourceType; All other wizard pages are identical regardless of the SourceType. At this point,
FlipDB has enough information to minimally process the data source and create a temporary FlipDB
table, which it does when you press "Next" to advance to the next page. This temporary table is
called the "raw source table". In the case of a CSV file, all columns of this temporary table will
be Char, as a CVS file has no additional type information. Other data sources may have type
information, and the temporary table will respect these types to the extent possible.

### Step 3: Specify the Target and Reference Tables

While FlipDB is now ready to apply a transformation to the raw source table, it is helpful to first
know the target table. If the target table exists, then FlipDB knows the required columns and data
types and can assist the user in transforming the raw source to match up with the target. If the
specified target table does not exist, then a "reference table" may be specified which performs
the same function. The reference table is used only as guide and is not updated or altered in any way.

### Step 4: Associate source columns or expressions with target columns.

Once the raw source table is available, and the target table is specified, a transformation must be
applied to the source so that it corresponds with the target. This involves mapping source column
names to target column names, modifying the type or values of source columns, writing expressions
to create new columns, and discarding unneeded source columns. This transformation is accomplished
by a simple, standard FlipDB query on the raw source table. Page 4 of the wizard displays
Transformation property of the associated Import object, which is used as the Select property of
this query. The query is executed when the user advances to the next page. The result of the query
is another temporary table known as the "transformed source table".

### Step 5: Specify Keys

If the target file does not exist, then this page asks you to specify the primary key column. If a
reference table is specified, the reference table's primary key column name will be suggested, but
any column with unique values may be specified, including the built-in auto-incrementing column
AUTOKEY. If the target file does exist, then this page asks you to specify a "lookup key". The
lookup key may consist of zero or more column names. All of these columns must exist in the
transformed source table and                                                          the target
table. The most common case is for the lookup key to be the primary key of the target table, but
it is often useful to look up rows based on an alternate key, or even other non-unique column or
column combinations. If the lookup key contains no column names, then all rows in the transformed
source will be considered "new" (In general, and empty lookup key is used only when the target
table is auto-keyed.) When the lookup key is specified and the Next button is pressed, FlipDB
performs an analysis of key values in the transformed source and in the target table, the results
of which are presented on the next wizard page.

### Step 6: Review Statistics, and Specify Actions

This page displays all of the relevant statistics about the raw source table and the transformed
source table, and how they compare to the target table. With this information in hand is you are
asked to specify how the target should be updated. If the target does not exist the only relevant
action is to create and load it. If the target does exist, then there are three options: Add new
rows, update existing rows and add new columns. This is the final page before FlipDB actually
modifies the target table or creates the target table if it does not exist. It is the last page at
which you may cancel the wizard or go backwards in the wizard.

