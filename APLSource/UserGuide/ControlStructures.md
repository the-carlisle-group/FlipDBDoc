# Control Structures

FlipDB provides a variety of ways to control the flow of execution, of a script, including
branching and looping. Control structures are  implemented by keywords preceded by a colon. For
example, the If  statement may be used to create a database only if it does not exist:

~~~
      S=Scripting.Server
      :If not S.DatabaseExists 'SandP'
          D=S.BuildSupplierAndParts ''
      :EndIf
~~~

Adding an Else clause makes it easy to retrieve a database if it exists, or create it if it does not:

~~~
      :If S.DatabaseExists 'SandP
           D=S.OpenDatabase 'SandP'
      :Else
          D=S.BuildSupplierAndParts ''
      :EndIf
~~~

The For statement may be used to iterate over a set of values. The values may be the items of a column:

~~~
      J=integers 10
      :For I :In J
          I
      :EndFor
~~~

The value of the control variable, in the above case I, changes each iteration of the loop. It is
possible to iterate over the items of an array of objects:

~~~
      D=Scripting.Server.OpenDatabase 'SandP'
      :For T :In D.Tables
          T
      :EndFor
~~~

