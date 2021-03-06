# Statement Analysis

A statement is a Boolean valued expression, 0 for false, 1 for true. For example:

~~~
      1 2 3 4 5 > 3
┌───────┐
↓0      │
│0      │
│0      │
│1      │
│1      │
└Boolean┘
~~~

A statement set is one or statements. It is often useful to group a query with a statement set
rather than the unique values of a column. Consider the parts table from the suppliers and parts database:

~~~
      t=Scripting.Server.Get 'Databases/SandP/P'
      t
── SandP.P ────────────────────────────────────────────
 ┌PNO────┐  ┌PNAME──┐  ┌COLOR──┐  ┌WEIGHT┐  ┌CITY────┐
 ↓P1     │  ↓Nut    │  ↓Red    │  ↓12    │  ↓London  │
 │P2     │  │Bolt   │  │Green  │  │17    │  │Paris   │
 │P3     │  │Screw  │  │Blue   │  │17    │  │Oslo    │
 │P4     │  │Screw  │  │Red    │  │14    │  │London  │
 │P5     │  │Cam    │  │Blue   │  │12    │  │Paris   │
 │P6     │  │Cog    │  │Red    │  │19    │  │London  │
 └Char(2)┘  └Char(5)┘  └Char(5)┘  └Int8──┘  └Char(10)┘
── 6 rows by 5 columns ────────────────────────────────
~~~

Now consider a query splitting the rows into 3 groups. The first group contains red parts with
weight greater than or equal to 14:

~~~
      q=t.Query ''
      q.AddGroupBy 'Red14Plus' '(COLOR in "Red") and WEIGHT >= 14'
──────────────────────────────────────────────────
 ┌Name─────┐  ┌Expression───────────────────────┐
 ↓Red14Plus│  ↓(COLOR in 'Red') and WEIGHT >= 14│
 └Char(9)──┘  └Char(33)─────────────────────────┘
── 1 rows by 2 columns ───────────────────────────
~~~

The second group contains red part with weight less than 14:

~~~
      q.AddGroupBy 'RedUnder14' '(COLOR in "Red") and WEIGHT < 14'
───────────────────────────────────────────────────
 ┌Name──────┐  ┌Expression───────────────────────┐
 ↓Red14Plus │  ↓(COLOR in 'Red') and WEIGHT >= 14│
 │RedUnder14│  │(COLOR in 'Red') and WEIGHT < 14 │
 └Char(10)──┘  └Char(33)─────────────────────────┘
── 2 rows by 2 columns ────────────────────────────
~~~

And the third group contains all other parts:

~~~
      q.AddGroupBy 'OtherColors' 'not COLOR in "Red"'
────────────────────────────────────────────────────
 ┌Name───────┐  ┌Expression───────────────────────┐
 ↓Red14Plus  │  ↓(COLOR in 'Red') and WEIGHT >= 14│
 │RedUnder14 │  │(COLOR in 'Red') and WEIGHT < 14 │
 │OtherColors│  │not COLOR in 'Red'               │
 └Char(11)───┘  └Char(33)─────────────────────────┘
── 3 rows by 2 columns ─────────────────────────────
~~~

We have now defined the GroupBy property of the query to be a statement set. Let's execute the
query and see the results:

~~~
      q.Execute 0
───────────────────────────
 ┌Partition──┐  ┌RowCount┐
 ↓Red14Plus  │  ↓2       │
 │RedUnder14 │  │1       │
 │OtherColors│  │3       │
 └Char(11)───┘  └Int8────┘
── 3 rows by 2 columns ────
~~~

This statement set has been carefully written to split the rows into mutually exclusive groups.
When, like this, statements are written with the intent of being exhaustive and mutually
exclusive, we call the statement set a **partition set**. This is the general use when we don't have
an existing column that groups the rows as we want. It is sometimes the case, though, that we
can't or don't want to define mutually exclusive groups. When a statement set is expected to have
overlapping groups and be non-exhaustive we refer to it as a **condition set**. One use of a condition
set is to identify data quality problems, like missing values, inappropriate values, or
inconsistent values. Consider a new query with the first group specified as red parts:

~~~
      q=t.Query ''
      q.AddGroupBy 'Red' 'COLOR in "Red"'
─────────────────────────────
 ┌Name───┐  ┌Expression────┐
 ↓Red    │  ↓COLOR in 'Red'│
 └Char(3)┘  └Char(14)──────┘
── 1 rows by 2 columns ──────
~~~

The second group as parts weighing 14 or greater:

~~~
      q.AddGroupBy 'Weight14Plus' 'WEIGHT >=14'
──────────────────────────────────
 ┌Name────────┐  ┌Expression────┐
 ↓Red         │  ↓COLOR in 'Red'│
 │Weight14Plus│  │WEIGHT >=14   │
 └Char(12)────┘  └Char(14)──────┘
── 2 rows by 2 columns ───────────
~~~

And the third group parts located in Paris:

~~~
      q.AddGroupBy 'Paris' 'CITY in "Paris"'
───────────────────────────────────
 ┌Name────────┐  ┌Expression─────┐
 ↓Red         │  ↓COLOR in 'Red' │
 │Weight14Plus│  │WEIGHT >=14    │
 │Paris       │  │CITY in 'Paris'│
 └Char(12)────┘  └Char(15)───────┘
── 3 rows by 2 columns ────────────
~~~

Any or all of these expressions may overlap to one degree or another. However, by default, FlipDB
forces mutually exclusive groups: each statement is applied in order, and the rows that it selects
are removed for consideration by subsequent statements:

~~~
      q.Execute 0
────────────────────────────
 ┌Partition───┐  ┌RowCount┐
 ↓Red         │  ↓3       │
 │Weight14Plus│  │2       │
 │Paris       │  │1       │
 └Char(12)────┘  └Int8────┘
── 3 rows by 2 columns ─────
~~~

This behavior may be overridden by setting the AllowOverlappingGroups property to 1, allowing any
particular row to belong to multiple groups. Now the groups will not be mutually exclusive:

~~~
      q.AllowOverlappingGroups=1
      q.Execute 0
────────────────────────────
 ┌Partition───┐  ┌RowCount┐
 ↓Red         │  ↓3       │
 │Weight14Plus│  │4       │
 │Paris       │  │2       │
 └Char(12)────┘  └Int8────┘
── 3 rows by 2 columns ─────
~~~

Note the sum of the RowCount column is 9, or 3 more than the total number of rows in the table. We
can add a total row to the query:

~~~
      q.IncludeTotalRow=1
      q.Execute 0
────────────────────────────
 ┌Partition───┐  ┌RowCount┐
 ↓Red         │  ↓3       │
 │Weight14Plus│  │4       │
 │Paris       │  │2       │
 │            │  │6       │
 └Char(12)────┘  └Int8────┘
── 4 rows by 2 columns ─────
~~~

The IncludeTotalRow property is applied without regard to the AllowOverlappingGroups property, and
is defined as the set union of all the groups. The row counts of the individual groups will sum to
the row count of the total group only if there are no overlapping groups.

By definition, grouping by a statement set may not select all of the rows in the table or under the
particular where clause; some rows may not fall into any group. (This is in contrast to
grouping by the unique values of a column, in which all rows are always represented.) Let's define
a new query where the first group is parts in Paris:

~~~
      q=t.Query ''
      q.AddGroupBy 'London' 'CITY in "Paris"'
──────────────────────────────
 ┌Name───┐  ┌Expression─────┐
 ↓London │  ↓CITY in 'Paris'│
 └Char(6)┘  └Char(15)───────┘
── 1 rows by 2 columns ───────
~~~

The second group is blue parts:

~~~
      q.AddGroupBy 'Blue' 'COLOR in "Blue"'
──────────────────────────────
 ┌Name───┐  ┌Expression─────┐
 ↓London │  ↓CITY in 'Paris'│
 │Blue   │  │COLOR in 'Blue'│
 └Char(6)┘  └Char(15)───────┘
── 2 rows by 2 columns ───────
~~~

And the third group is parts of type bolt:

~~~
      q.AddGroupBy 'Bolt' 'PNAME in "Bolt"'
──────────────────────────────
 ┌Name───┐  ┌Expression─────┐
 ↓London │  ↓CITY in 'Paris'│
 │Blue   │  │COLOR in 'Blue'│
 │Bolt   │  │PNAME in 'Bolt'│
 └Char(6)┘  └Char(15)───────┘
── 3 rows by 2 columns ───────
~~~

We will allow overlapping groups, and include a total row, and then run the query:

~~~
      q.AllowOverlappingGroups=1
      q.IncludeTotalRow=1
      q.Execute 0
─────────────────────────
 ┌Partition┐  ┌RowCount┐
 ↓London   │  ↓2       │
 │Blue     │  │2       │
 │Bolt     │  │1       │
 │         │  │3       │
 └Char(6)──┘  └Int8────┘
── 4 rows by 2 columns ──
~~~

We see that only a total of 3 rows are selected (so we must have some overlapping groups). We can
get the remaining unselected rows, thus forcing an exhaustive set of groups, by adding a statement
consisting the key word [StatementSetComplement]:

~~~
      q.AddGroupBy 'Other' '[StatementSetComplement]'
───────────────────────────────────────
 ┌Name───┐  ┌Expression──────────────┐
 ↓London │  ↓CITY in 'Paris'         │
 │Blue   │  │COLOR in 'Blue'         │
 │Bolt   │  │PNAME in 'Bolt'         │
 │Other  │  │[StatementSetComplement]│
 └Char(6)┘  └Char(24)────────────────┘
── 4 rows by 2 columns ────────────────
~~~

Executing the query we see that the Other category selects the remaining 3 rows of the table:

~~~
      q.Execute 0
─────────────────────────
 ┌Partition┐  ┌RowCount┐
 ↓London   │  ↓2       │
 │Blue     │  │2       │
 │Bolt     │  │1       │
 │Other    │  │3       │
 │         │  │6       │
 └Char(6)──┘  └Int8────┘
── 5 rows by 2 columns ──
~~~

There are two additional key words applicable to statement sets. First, [StatementSetUnion] creates
a group consisting of the union of all groups) (not including any groups defined be other key words):

~~~
      q.AddGroupBy 'Union' '[StatementSetUnion]'
───────────────────────────────────────
 ┌Name───┐  ┌Expression──────────────┐
 ↓London │  ↓CITY in 'Paris'         │
 │Blue   │  │COLOR in 'Blue'         │
 │Bolt   │  │PNAME in 'Bolt'         │
 │Other  │  │[StatementSetComplement]│
 │Union  │  │[StatementSetUnion]     │
 └Char(6)┘  └Char(24)────────────────┘
── 5 rows by 2 columns ────────────────
~~~

And second, [StatementSetIntersection] creates a group consisting of the intersection of all
groups) (again not including any groups defined be other key words):

~~~
      q.AddGroupBy 'Intersection' '[StatementSetIntersection]'
──────────────────────────────────────────────
 ┌Name────────┐  ┌Expression────────────────┐
 ↓London      │  ↓CITY in 'Paris'           │
 │Blue        │  │COLOR in 'Blue'           │
 │Bolt        │  │PNAME in 'Bolt'           │
 │Other       │  │[StatementSetComplement]  │
 │Union       │  │[StatementSetUnion]       │
 │Intersection│  │[StatementSetIntersection]│
 └Char(12)────┘  └Char(26)──────────────────┘
── 6 rows by 2 columns ───────────────────────
~~~

Let's see what the numbers are for these:

~~~
      q.Execute 0
────────────────────────────
 ┌Partition───┐  ┌RowCount┐
 ↓London      │  ↓2       │
 │Blue        │  │2       │
 │Bolt        │  │1       │
 │Other       │  │3       │
 │Union       │  │3       │
 │Intersection│  │0       │
 │            │  │6       │
 └Char(12)────┘  └Int8────┘
── 7 rows by 2 columns ─────
~~~

Here we see that there are 3 parts that are either in London, or blue, or bolts (the union), but
that there are 0 blue bolts in London (the intersection). Note that the InlcudeTotalRow property
is defined as the union of all the groups and is thus somewhat similar in concept to the
[StatementSetUnion]  key word. The difference is that IncludeTotalRow includes the group defined
by [StatementSetComplement], but [StatementSetUnion] does not.

