# The Import Object and Wizard

FlipDB provides an Import object and an associated Import Wizard for importing data from a variety
of sources into FlipDB. The Import object has properties that define the data source, the
transformation that should be applied to the data source, the target table, and, if the target
table exists, how it should be updated. Once these properties have been set, the Execute method is
called to run the import and create or update the destination table.

The Import Wizard allows for a visual, incremental approach to using the Import object where you
are asked to specify only the necessary properties for a particular data source, one (or a few) at
a time, in a logical order, with feedback at each step. This feedback helps you make the
appropriate choice for the various properties. For example, you are asked to specify whether new
rows should be added an existing table only after specifying the lookup column and, given that
lookup column, being informed of how many rows match, how many are unique, and how many rows are
new. Once you have stepped through the wizard, the associated Import object is saved and may be
re-executed with the wizard or loaded and executed in a script.

The general approach of the Import Object and Wizard may be divided into three major steps. (The
actual wizard is divided into more steps, which are detailed in the following section) First,
bring in the data source as-is, with the fewest possible questions asked, into a temporary table.
Many properties of the Import object are data source specific and are only relevant at this stage.
For example, in the case of a CSV file data source, this requires specifying the FileName,
→[*.Delimiter], →[*.TextQualifier] and →[*.HasHeader] properties. Once this stage is complete,
the type of data source is immaterial.

Second, transform this temporary table as necessary by renaming columns, recasting data types,
mapping column values and creating new columns. This is accomplished with the →[*.Transformation]
property, which is an expression set used as the Select property of a Query that is then applied
to the temporary table to produce the transformed table.

Finally, specify the target table, lookup keys, and how the target should be updated, including
whether or not new rows and columns should be added and existing rows updated.

