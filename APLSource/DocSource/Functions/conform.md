# conform

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Replicate and or reorder the rows of a datatable or propertyspace to
conform to a key. Table lookup. {.purpose}

~~~
R=X conform Y Z [B]
~~~

Y is a datatable (or propertyspace that may be transposed to a datatable),
or a string that names a table and is a suitable argument for →[getTable].
Z is the name of one or more columns in Y that make up a key.
X is one or more columns that correspond to the names in Z in number and type.
R is a datatable (or propertyspace if Y is a propertyspace) composed of the rows
of Y selected, ordered and replicated where necessary to correspond to X.
In other words, R is Y made to conform to X.

B is an optional boolean flag to keep duplicate rows in Y.
The default is 0, in which case duplicates in Y are ignored, effectively discarded.
The columns in result R will all be simple columns. This enforces a one-to-one or many-to-one relationship
between key X and the key in Y. If B is 1, then duplicates are kept, and all but the key columns
in result R are partitioned. This allows a one-to-many or many-to-many relationship
between the key X and the key in Y.

## Examples:

With a datatable:

~~~
      DT
─────────────────────────────────────────────
 ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌SDATE─────┐
 ↓S1     │  ↓P1     │  ↓300  │  ↓2008/10/27│
 │S1     │  │P2     │  │200  │  │2008/08/19│
 │S1     │  │P3     │  │400  │  │2008/05/18│
 │S1     │  │P4     │  │200  │  │2007/08/18│
 │S1     │  │P5     │  │100  │  │2007/08/17│
 │S1     │  │P6     │  │100  │  │2009/12/16│
 │S2     │  │P1     │  │300  │  │2009/07/31│
 │S2     │  │P2     │  │400  │  │2009/12/17│
 │S3     │  │P2     │  │200  │  │2009/07/24│
 │S4     │  │P2     │  │200  │  │2009/10/03│
 │S4     │  │P4     │  │300  │  │2009/06/28│
 │S4     │  │P5     │  │400  │  │2009/11/04│
 └Char(2)┘  └Char(2)┘  └Int16┘  └Date──────┘
── 12 rows by 4 columns ─────────────────────

      'P5,P2,P9,P1,P5' conform DT 'PNO'
─────────────────────────────────────────────
 ┌SNO────┐  ┌PNO────┐  ┌QTY──┐  ┌SDATE─────┐
 ↓S1     │  ↓P5     │  ↓100  │  ↓2007/08/17│
 │S1     │  │P2     │  │200  │  │2008/08/19│
 │       │  │       │  │0    │  │          │
 │S1     │  │P1     │  │300  │  │2008/10/27│
 │S1     │  │P5     │  │100  │  │2007/08/17│
 └Char(2)┘  └Char(2)┘  └Int16┘  └Date──────┘
── 5 rows by 4 columns ──────────────────────

      'P5,P2,P9,P1,P5' conform DT 'PNO' 1
──────────────────────────────────────────────────────────────────────────────────────────────────
 ┌SNO──────────┐  ┌PNO────┐  ┌QTY──────────────┐  ┌SDATE────────────────────────────────────────┐
 ↓[S1,S4]      │  ↓P5     │  ↓[100,400]        │  ↓[2007/08/17,2009/11/04]                      │
 │[S1,S2,S3,S4]│  │P2     │  │[200,400,200,200]│  │[2008/08/19,2009/12/17,2009/07/24,2009/10/03]│
 │[]           │  │       │  │[]               │  │[]                                           │
 │[S1,S2]      │  │P1     │  │[300,300]        │  │[2008/10/27,2009/07/31]                      │
 │[S1,S4]      │  │P5     │  │[100,400]        │  │[2007/08/17,2009/11/04]                      │
 └Char(2)──────┘  └Char(2)┘  └Int16────────────┘  └Date─────────────────────────────────────────┘
── 5 rows by 4 columns ───────────────────────────────────────────────────────────────────────────
~~~
With a propertyspace:

~~~
        P=newPropertySpace ''
        P.State='NY,CT,NJ,PA'
        P.Amount=100 200 300 400
        K='NY,NY,CA,PA,NJ,NJ'
        CP=K conform P 'State'
        K CP.State CP.Amount
 ┌───────┐  ┌State──┐  ┌Amount┐
 ↓NY     │  ↓NY     │  ↓100   │
 │NY     │  │NY     │  │100   │
 │CA     │  │       │  │0     │
 │PA     │  │PA     │  │400   │
 │NJ     │  │NJ     │  │300   │
 │NJ     │  │NJ     │  │300   │
 └Char(2)┘  └Char(2)┘  └Int16─┘
~~~

