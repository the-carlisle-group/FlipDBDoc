# Delete Method

Applies to:{.prefix}

→[##.##.Table]{.info}

This method deletes rows in a table based on primary key values.

The argument is composed of 1 item:

|-|-|
|1|KeyValues|DataColumn or DataTable|

The result is composed of 1 item:

|-|-|
|1|DeletedRowCount|Integer|

Key values may be provided directly as a simple DataColumn. For example:

~~~
      S=Scripting.Server
      D=S.BuildSuppliersAndParts ''
      T=D.GetTable 'SP'
      T.Delete 1 2
┌────┐
│2   │
└Int8┘
~~~

Key values may also be provided in a DataTable:

~~~
     DT = T.ExecuteQuery 'QTY gt 300' ''
     DT
── Key:AUTOKEY ─────────────────────────────────────────
 ┌AUTOKEY┐  ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌SDATE─────┐
 ↓3      │  ↓S1     │  ↓P3     │  ↓400  │  ↓2008-05-18│
 │8      │  │S2     │  │P2     │  │400  │  │2009-12-17│
 │12     │  │S4     │  │P5     │  │400  │  │2009-11-04│
 └Int8───┘  └Char(2)┘  └Char(2)┘  └Int16┘  └Date──────┘
── 3 rows by 5 columns ─────────────────────────────────
      T.Delete DT
┌────┐
│3   │
└Int8┘
      T
── SandP.SP ────────────────────────────────────────────
 ┌AUTOKEY┐  ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌SDATE─────┐
 ↓4      │  ↓S1     │  ↓P4     │  ↓200  │  ↓2007-08-18│
 │5      │  │S1     │  │P5     │  │100  │  │2007-08-17│
 │7      │  │S2     │  │P1     │  │300  │  │2009-07-31│
 │9      │  │S3     │  │P2     │  │200  │  │2009-07-24│
 │10     │  │S4     │  │P2     │  │200  │  │2009-10-03│
 │11     │  │S4     │  │P4     │  │300  │  │2009-06-28│
 └Int8───┘  └Char(2)┘  └Char(2)┘  └Int16┘  └Date──────┘
── 6 rows by 5 columns ─────────────────────────────────
~~~

No rows will be deleted unless all key values exist. No rows will be deleted if there are any
outstanding references to any single specified row. In both case errors are signalled.

See also the →[*.Table.MethodList.DeleteWhere] method.

Note that in FlipDB, deletes are logical - no data is destroyed or overwritten. This allows for
historical queries, but it also means that deleting rows does not free up disk space. In fact, in
FlipDB deleting rows is as expensive as updating rows, and takes up additional space. In certain
circumstances, it is useful to physically delete rows, which may be accomplished with the
→[*.Table.MethodList.Purge] and →[*.Table.MethodList.PurgeWhere] methods.

