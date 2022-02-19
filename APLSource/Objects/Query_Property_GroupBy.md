# GroupBy Property

Applies to:{.prefix}

→[##.##.Query]{.info}

The GroupBy property specifies one or more columns or expressions in the starting table to be
used to group data, reducing the number of rows in the query result. In addition, grouping may be
done by boolean expressions (a statement set), explicitly defining each group, rather than the
using the unique values of a column.

The GroupBy property is formally a DataTable with Name and Expression columns (an expression set),
and may be specified as such, but it may also be specified as a simple column name:

~~~
      Q.GroupBy='COLOR'
      Q.GroupBy
──────────────────────────────
 ┌Name────────┐  ┌Expression┐
 ↓GroupByCOLOR│  ↓COLOR     │
 └Char(12)────┘  └Char(5)───┘
── 1 rows by 2 columns ───────
~~~

In this case, FlipDB automatically assigns a name for the group. The name of the group may be
explicitly set:

~~~
       Q.GroupBy='PartColor' 'COLOR'
       Q.GroupBy
───────────────────────────
 ┌Name─────┐  ┌Expression┐
 ↓PartColor│  ↓COLOR     │
 └Char(9)──┘  └Char(5)───┘
── 1 rows by 2 columns ────
~~~

Regardless of how the GroupBy property is set, it always returns a DataTable.

Date and numeric columns may be ranged by applying the →range function to a column:

~~~
        Q.GroupBy='PartWeight' '10 20 4 range WEIGHT'
        Q.GroupBy
──────────────────────────────────────
 ┌Name──────┐  ┌Expression──────────┐
 ↓PartWeight│  ↓10 20 4 range WEIGHT│
 └Char(10)──┘  └Char(20)────────────┘
── 1 rows by 2 columns ───────────────
~~~

The helper method AddGroupBy incrementally adds a grouping to the GroupBy property:

~~~
      Q.GroupBy='PartWeight' 'WEIGHT'
      Q.AddGroupBy 'PartColor' 'COLOR'
────────────────────────────
 ┌Name──────┐  ┌Expression┐
 ↓PartWeight│  ↓WEIGHT    │
 │PartColor │  │COLOR     │
 └Char(10)──┘  └Char(6)───┘
── 2 rows by 2 columns ─────
~~~

## Grouping by Statement Set

If all of the expressions in the GroupBy property are boolean expressions, then the GroupBy
property is treated as a statement set with each expression defining a group. The
→[AllowOverlappingGroups] property determines if groups are forced to be mutually exclusive or
not. If AllowOverlappingGroups is set to 0, then each group will only contain rows that are not
included in previously defined groups. In this case the order that groups are defined GroupBy
property is important.

In addition, three key words may be used to form groups related to those defined by explicit
statements.The key word [StatementSetComplement] is be used as an expression to select all
remaining rows. The key word [StatementSetUnion] selects all rows that fall into any group  (the
union of all groups), and the key [StatementSetIntersection] selects rows that fall in every group
(the intersection of all groups). This last key word operates independently of the
AllowOverlappingGroups property. In other words it always includes the rows that would be in every
group if AllowOverlappingGroups was set to 1 as otherwise it would always contain zero rows.

