# Assign Method

Applies to:{.prefix}

→[##.##.Database]{.info}

This method performs multiple assignments to the database.

The argument is composed of 1 item:

|-|-|
|1|Assignments|Object array of assignments|

The result is composed of 1 item:

|-|-|
|1|ReturnCode|0|

For example, to execute assignment A0:

~~~
      D.Assign A0
~~~

And to execute assignments A0, A1 and A2:

~~~
      D.Assign A0 A1 A2
~~~

Note that the order of assignments in the argument is important if there are any dependent
assignments in the collection.

The Assign method guarantees that either all assignments are executed or none are executed.

