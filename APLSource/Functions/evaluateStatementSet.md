# evaluateStatementSet

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Misc{.info}

Evaluate a statement set.{.purpose}

~~~
R=evaluateStatementSet Y
~~~

Y is a statement set or a string containing the fully qualified path and name
of a saved statement set. Y must contain an Expression column of boolean valued
expressions.

R is a partioned boolean column. Each partition is of length N, where N is
the number of statements.

The Boolean result R is often analyzed using the aggregate functions →[any], →[all], and →[none].

See also: →[evaluateCaseStatement] →[evaluateExpressionSet]


