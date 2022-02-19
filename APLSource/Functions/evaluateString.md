
# evaluateString

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Misc{.info}

Evaluate expressions in a string. String Interpolation.{.purpose}

~~~
R=evaluateString X
~~~

X is Char. R is Char, with expressions in X evaluated, formatted, and converted
to character data. Expression are denoted by pairs of curly braces.
An expression may contain an optional format string, seprated by a colon.'
String interpolation is useful to avoid repeated string catenation.
This function is primarily useful in scripts, but it will work in
any component of  a a query as well.

The toChar function is automatically called on the result of any expression.

### Examples

Braces are used to denote an expression, usually just a simple name:

~~~
      First='Friedrich'
      Last='Hayek'
      evaluateString 'My first name is {First} and my last name is {Last}'
┌────────────────────────────────────────────────────┐
│My first name is Friedrich and my last name is Hayek│
└Char(52)────────────────────────────────────────────┘
~~~

Expressions must resolve to a simple string. The toLiteral function
is useful when building up expressions for queries:'

~~~
      States=toLiteral 'NY,NJ,CT'
      Rate=3.125
      evaluateString '(State in {States}) and Rate gt {Rate}'
┌───────────────────────────────────────┐
│(State in 'NY,NJ,CT') and Rate gt 3.125│
└Char(39)───────────────────────────────┘
 ~~~

Usually expressions are just simple names, but they can include any full
FlipDB expression:

~~~
      IntsTo100=1+integers 100
      evaluateString 'The sum of the integers from 1 to 100 is {sum IntsTo100}'
┌─────────────────────────────────────────────┐
│The sum of the integers from 1 to 100 is 5050│
└Char(45)─────────────────────────────────────┘
~~~

An expression may contain an optional format string, separated by a colon:

~~~
      Balance=3141.59
      evaluateString 'Your current balance is {Balance:P<$>LCF10.2}'
┌─────────────────────────────────┐
│Your current balance is $3,141.59│
└Char(34)─────────────────────────┘
~~~

