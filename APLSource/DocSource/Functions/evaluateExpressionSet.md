# evaluateExpressionSet

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Misc{.info}

Evaluate an expression set as a user defined function.{.purpose}

~~~
R=[X] evaluateExpressionSet Y
~~~

Y is an expression set or a string containing the fully qualified path and name
of a saved expression set. The expression set must contain a Name column and
an Expression column.

X is a property space containing the required inputs for evaluating the expression set.
It is optional.

R is the result of the last expression in Y. For this reason, the Name associated
with this last expression inside the expression set is not meaningful. This last expression
may result in a column or a property space, which allows for multiple values to be returned.

Y is evaluated in a local, protected space. Any name assigned in Y will not affect the calling
environment.

If X is provided, the only names and values that Y can see are those that are provided in X.
This allows for a functional approach. It is best to use this approach if the number of inputs
is relatively small. This makes reading the calling code easier as the inputs are explicit.

If X is not provided, then Y can see everything that is available in the calling environment,
including previously defined names, and all of the tables and columns of the database.  However
names assigned in Y will still not affect the calling environment. This approach is better
if the expression set requires many inputs that you can expect to be properly defined in the
database.

See also: →[evaluateStatementSet] →[evaluateCaseStatement]

### Example 1
In this example a left argument is provided. The evaluateExpressionSet function
may then only see the names and values explicitly provided.
~~~
      xs=newExpressionSet 'Name,Expression'
      _=xs.AddRows 'Product' 'Weight times Factor'
      _=xs.AddRows 'Plus5' 'Product + 5'
      s=Scripting.Server
      s.Put '/Objects/MySet1' xs
──────────────────────────────────
 ┌Name───┐  ┌Expression─────────┐
 ↓Product│  ↓Weight times Factor│
 │Plus5  │  │Product + 5        │
 └Char(7)┘  └Char(19)───────────┘
── 2 rows by 2 columns ───────────
      t=s.Get '/Databases/SandP/P'
      q=t.Query ''
      _=q.AddColumn 'PNO'
      _=q.AddColumn 'WEIGHT'
      _=q.AddColumn 'Input' 'newPropertySpace 0'
      _=q.AddColumn 'Input.Weight' 'WEIGHT / 2'
      _=q.AddColumn 'Input.Factor' '1.5'
      _=q.AddColumn 'Result' 'Input evaluateExpressionSet "/Objects/MySet1"'
      q.Execute 0
── Key:PNO ────────────────────────────────────────────────────
 ┌PNO────┐  ┌WEIGHT┐  ┌Input.Weight┐  ┌Input.Factor┐  ┌Result┐
 ↓P1     │  ↓12    │  ↓6           │  │1.5         │  ↓14    │
 │P2     │  │17    │  │8.5         │  └Dec(1)──────┘  │17.75 │
 │P3     │  │17    │  │8.5         │                  │17.75 │
 │P4     │  │14    │  │7           │                  │15.5  │
 │P5     │  │12    │  │6           │                  │14    │
 │P6     │  │19    │  │9.5         │                  │19.25 │
 └Char(2)┘  └Int8──┘  └Float───────┘                  └Float─┘
── 6 rows by 5 columns ────────────────────────────────────────
~~~

### Example 2
This example uses the same expression set as above, but no
left argument of explicit inputs is provided. In this case, evaluateExpressionSet
sees all of the names defined in the query (as well as all of the tables and columns in
the database).

~~~
      q=t.Query ''
      _=q.AddColumn 'PNO'
      _=q.AddColumn 'Weight' 'WEIGHT / 2'
      _=q.AddColumn 'Factor' '1.5'
      _=q.AddColumn 'Result' 'evaluateExpressionSet "/Objects/MySet1"'
      q.Execute 0
── Key:PNO ──────────────────────────────
 ┌PNO────┐  ┌Weight┐  ┌Factor┐  ┌Result┐
 ↓P1     │  ↓6     │  │1.5   │  ↓14    │
 │P2     │  │8.5   │  └Dec(1)┘  │17.75 │
 │P3     │  │8.5   │            │17.75 │
 │P4     │  │7     │            │15.5  │
 │P5     │  │6     │            │14    │
 │P6     │  │9.5   │            │19.25 │
 └Char(2)┘  └Float─┘            └Float─┘
── 6 rows by 4 columns ──────────────────

~~~
