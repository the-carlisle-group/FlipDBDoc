
# evaluateCaseStatement

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Misc{.info}

Evaluate a statement set.{.purpose}

~~~
R=[X] evaluateCaseStatement Y
~~~

Y is a statement set or a string containing the fully qualified path and name
of a saved statement set. Y must contain an Expression column of boolean valued
expressions. It must contain either (or both) a Name column, with a valid FlipDB
name, or a Value column of expressions. If Value does not exist, the Name column
is used in its place.

X is a boolean flag indicating that overlapping statements
are allowed (1) or not (0). The default is 0.

If X is 0, then R is a simple column, composed of Value where the first corresponding
Expression is true. In this scenario, evaluate is like a case statement, or a repeated
if/then/else statement.

If X is 1, then R is a partitioned column, composed of Value where each
corresponding Expression is true.

Note that the statement set may be a "condition set" (the statements are not
intended to mutually exclusive) or a partition set (the statements are actually
mutually exclusive or intended to be).

See also: →[evaluateStatementSet] →[evaluateExpressionSet]

### Example 1

In this example, the statements are not explicitly written to be mutually exclusive, but
we intend them to be so. As we have not set the left argument to 1 (it defaults to 0) mutual
exclusivity is enforced. This is essentially a case or if/then/else statement and may be read
"if Expression then Value". Note that the Value column is provided in the expression set
and is itself an expression, in this case just simple quoted strings.

~~~
      xs=newExpressionSet 'Expression,Value'
      j=xs.AddRows 'WEIGHT le 12' '"Light"'
      j=xs.AddRows 'WEIGHT le 15' '"Medium"'
      j=xs.AddRows 'WEIGHT le 20' '"Heavy"'
      s=Scripting.Server
      s.Put '/Objects/ExpSet1' xs
────────────────────────────
 ┌Expression──┐  ┌Value───┐
 ↓WEIGHT le 12│  ↓"Light" │
 │WEIGHT le 15│  │"Medium"│
 │WEIGHT le 20│  │"Heavy" │
 └Char(12)────┘  └Char(8)─┘
── 3 rows by 2 columns ─────

      t=s.Get '/Databases/SandP/P'
      q=t.Query ''
      j=q.AddColumn 'PNO' 'PNO'
      j=q.AddColumn 'WEIGHT' 'WEIGHT'
      j=q.AddColumn 'Category' 'evaluateCaseStatement "/Objects/ExpSet1"'
      q.Execute 0
── Key:PNO ──────────────────────
 ┌PNO────┐  ┌WEIGHT┐  ┌Category┐
 ↓P1     │  ↓12    │  ↓Light   │
 │P2     │  │17    │  │Heavy   │
 │P3     │  │17    │  │Heavy   │
 │P4     │  │14    │  │Medium  │
 │P5     │  │12    │  │Light   │
 │P6     │  │19    │  │Heavy   │
 └Char(2)┘  └Int8──┘  └Char(6)─┘
── 6 rows by 3 columns ──────────

~~~

### Example 2

In this example, the statements are not meant to be mutually exclusive.
The query result shows the results of the evaluate function both with
overlapping statements and without overlapping statements.

~~~
      xs=newExpressionSet 'Name,Expression'
      j=xs.AddRows 'Red' 'COLOR in "Red"'
      j=xs.AddRows 'Heavy' 'WEIGHT gt 15'
      j=xs.AddRows 'London' 'CITY in "London"'
      s.Put '/Objects/ExpSet2' xs
───────────────────────────────
 ┌Name───┐  ┌Expression──────┐
 ↓Red    │  ↓COLOR in "Red"  │
 │Heavy  │  │WEIGHT gt 15    │
 │London │  │CITY in "London"│
 └Char(6)┘  └Char(16)────────┘
── 3 rows by 2 columns ────────
      q=t.Query ''
      j=q.AddColumn 'PNO'
      j=q.AddColumn 'PNO'
      q=t.Query ''
      j=q.AddColumn 'PNO'
      j=q.AddColumn 'First' '0 evaluateCaseStatement "/Objects/ExpSet2"'
      j=q.AddColumn 'All' '1 evaluateCaseStatement "/Objects/ExpSet2"'
      q.Execute 0
── Key:PNO ─────────────────────────────────
 ┌PNO────┐  ┌First──┐  ┌All───────────────┐
 ↓P1     │  ↓Red    │  ↓[Red,London]      │
 │P2     │  │Heavy  │  │[Heavy]           │
 │P3     │  │Heavy  │  │[Heavy]           │
 │P4     │  │Red    │  │[Red,London]      │
 │P5     │  │       │  │[]                │
 │P6     │  │Red    │  │[Red,Heavy,London]│
 └Char(2)┘  └Char(5)┘  └Char(6)───────────┘
── 6 rows by 3 columns ─────────────────────
~~~

### Example 3

In this example we compute zero or more factors based on the characteristics of a part,
and sum them up:

~~~
      xs=newExpressionSet 'Name,Expression,Value'
      j=xs.AddRows 'Red' 'COLOR in "Red"'  '.5'
      j=xs.AddRows 'Heavy' 'WEIGHT gt 15' '1.25'
      j=xs.AddRows 'London' 'CITY in "London"' '.25'
      s.Put '/Objects/ExpSet3' xs
──────────────────────────────────────────
 ┌Name───┐  ┌Expression──────┐  ┌Value──┐
 ↓Red    │  ↓COLOR in "Red"  │  ↓.5     │
 │Heavy  │  │WEIGHT gt 15    │  │1.25   │
 │London │  │CITY in "London"│  │.25    │
 └Char(6)┘  └Char(16)────────┘  └Char(4)┘
── 3 rows by 3 columns ───────────────────
      q=t.Query ''
      j=q.AddColumn 'PNO'
      j=q.AddColumn 'Factors' '1 evaluateCaseStatement "/Objects/ExpSet3"'
      j=q.AddColumn 'SumOfFactors' 'sum Factors'
      q.Execute 0
── Key:PNO ────────────────────────────────────
 ┌PNO────┐  ┌Factors─────────┐  ┌SumOfFactors┐
 ↓P1     │  ↓[0.50,0.25]     │  ↓0.75        │
 │P2     │  │[1.25]          │  │1.25        │
 │P3     │  │[1.25]          │  │1.25        │
 │P4     │  │[0.50,0.25]     │  │0.75        │
 │P5     │  │[]              │  │0.00        │
 │P6     │  │[0.50,1.25,0.25]│  │2.00        │
 └Char(2)┘  └Dec(2)──────────┘  └Dec(2)──────┘
── 6 rows by 3 columns ────────────────────────

~~~


