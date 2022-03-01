# Macros Property

Applies to:{.prefix}

→[##.##.Report]{.info}

The Macros property is an expression set that defines a set of names and expressions
used for text substitition throughout the report. Just before evaluating an expression,
FlipDB replaces a macro name with its corresponding expression. Macro names are defined
as standard FlipDB names, but referenced with a leading exclamation point (!).

For example, consider the macro expression set:

 |Name|Expression|
 |---|-----------|
 |`WAC`|`3 round weightedAverage Rate Balance`|
 |`Interest`|`Rate*Balance/1200`|
 |`HighLTV`|`LTV gt 80`|

An expression in a query in the report may then refer to `!WAC`, `!HighLTV`, or
`!Interest` and the correponding macro expression is substituted. For example:

|Name|Expression|
|---|-----------|
|`WAC`|`!WAC`|
|`HighLTVInt`|`sum !Interest where !HighLTV`|

Just before executing, the macros are expanded:

|Name|Expression|
|---|-----------|
|`WAC`|`3 round weightedAverage Rate Balance`|
|`HighLTVInt`|`sum (Rate*Balance/1200) where (LTV gt 80)`|

A macro is surrounded by parentheses when it is inserted into an expression.
Thus the macro value should be a complete expression, not a fragment.

A macro expression itself may reference macros. If a circular reference is detected, replacement
ceases.

Macros provide a way to avoid duplication, making reports and queries easier to maintain.
Note the fundamental difference between a global value and a macro: a global is evaluated
at the beginning of a report. The context of any particular query that references the
global value is irrelevant. A macro is the last-minute substituion of text in an expression which
is then evaluated in the relevant context.






