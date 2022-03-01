# WhereNot Property

Applies to:{.prefix}

→[##.##.Query]{.info}

There WhereNot property specifies one or more Boolean expressions used to filter the number of rows
in the starting table. It is similar to the Where clause in a SQL statement, but it allows you to
specify what you don't want rather than what you do want.

The WhereNot property is formally a DataTable with an Expression column (a Boolean expression set),
and may be specified as such, but it may also conveniently be specified as a simple string:

~~~
      Q.WhereNot='COLOR eq "Red"'
      Q.WhereNot
───────────────────────
 ┌Expression────┐
 ↓COLOR eq 'Red'│
 └Char(14)──────┘
── 1 rows by 1 columns
~~~

The helper method →[*.AddWhereNot] may be used to incrementally add a where clause:

~~~
      Q.AddWhereNot 'WEIGHT lt 14'
───────────────────────
 ┌Expression────┐
 ↓COLOR eq 'Red'│
 │WEIGHT lt 14  │
 └Char(14)──────┘
── 2 rows by 1 columns
~~~

Regardless of how the WhereNot property is specified, it always returns a DataTable.

If multiple expressions are provided, they are logically "or-ed" together. Thus the above is
equivalent to:

~~~
      Q.WhereNot='(COLOR eq "Red") or (WEIGHT lt 14)'
      Q.WhereNot
──────────────────────────────────────
 ┌Expression────────────────────────┐
 ↓(COLOR eq 'Red') or (WEIGHT lt 14)│
 └Char(35)──────────────────────────┘
── 1 rows by 1 columns ───────────────
~~~

Note that for readability, debugging, and performance, it is better to use more short expressions
than fewer long expressions.

