# getTable

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Gets the columns of a named table as a DataTable.{.purpose}

~~~
R=[Y] getTable X
~~~

X is a character string specifying the name of a table,
or the name of the database and table separated by a dot.
If the database name is omitted, the database is assumed by the query context.
In a script, the database must be specified.

Y is an optional boolean flag specifying whether or not to include system columns in the result.
It defaults to 0.

R is a DataTable.

See also →[transpose]

## Examples
~~~
      getTable 'SandP.P'
───────────────────────────────────────────────────────
 ┌PNO────┐  ┌PNAME──┐  ┌COLOR──┐  ┌WEIGHT┐  ┌CITY────┐
 ↓P1     │  ↓Nut    │  ↓Red    │  ↓12    │  ↓London  │
 │P2     │  │Bolt   │  │Green  │  │17    │  │Paris   │
 │P3     │  │Screw  │  │Blue   │  │17    │  │Oslo    │
 │P4     │  │Screw  │  │Red    │  │14    │  │London  │
 │P5     │  │Cam    │  │Blue   │  │12    │  │Paris   │
 │P6     │  │Cog    │  │Red    │  │19    │  │London  │
 └Char(2)┘  └Char(5)┘  └Char(5)┘  └Int8──┘  └Char(10)┘
── 6 rows by 5 columns ────────────────────────────────

      P=transpose getTable 'SandP.P'
      P
┌PropertySpace───────────┐
│ Name         Type      │
│ ---------    --------- │
│ PNO          Char      │
│ PNAME        Char      │
│ COLOR        Char      │
│ WEIGHT       Integer   │
│ CITY         Char      │
└────────────────────────┘

      P.CITY
┌CITY────┐
↓London  │
│Paris   │
│Oslo    │
│London  │
│Paris   │
│London  │
└Char(10)┘

     1 getTable 'SandP.P'
───────────────────────────────────────────────────────────────────────────────────────────────────────────────
 ┌TWID┐  ┌APPENDTYPE┐  ┌PNO────┐  ┌AUTOKEY┐  ┌PNAME──┐  ┌COLOR──┐  ┌WEIGHT┐  ┌CITY────┐  ┌TransDateTime──────┐
 ↓2   │  ↓0         │  ↓P1     │  ↓1      │  ↓Nut    │  ↓Red    │  ↓12    │  ↓London  │  ↓2019-01-09 09:21:43│
 │2   │  │0         │  │P2     │  │2      │  │Bolt   │  │Green  │  │17    │  │Paris   │  │2019-01-09 09:21:43│
 │2   │  │0         │  │P3     │  │3      │  │Screw  │  │Blue   │  │17    │  │Oslo    │  │2019-01-09 09:21:43│
 │2   │  │0         │  │P4     │  │4      │  │Screw  │  │Red    │  │14    │  │London  │  │2019-01-09 09:21:43│
 │2   │  │0         │  │P5     │  │5      │  │Cam    │  │Blue   │  │12    │  │Paris   │  │2019-01-09 09:21:43│
 │2   │  │0         │  │P6     │  │6      │  │Cog    │  │Red    │  │19    │  │London  │  │2019-01-09 09:21:43│
 └Int8┘  └Int8──────┘  └Char(2)┘  └Int8───┘  └Char(5)┘  └Char(5)┘  └Int8──┘  └Char(10)┘  └DateTime───────────┘
── 6 rows by 9 columns ────

~~~
