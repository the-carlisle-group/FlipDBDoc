# The Session

The FlipDB session is a window for entering and evaluating FlipDB expressions. Depending on the
setup, the session window may be the primary window and the first thing you see on starting
FlipDB. If you are in the Explorer, you may launch the session by going to the Tools/Session menu.

The session allows you to type an expression and press the <Enter> key  to evaluate it. Once the
expression is evaluated, the result is displayed. For example, the integer 5 is a valid FlipDB expression:

~~~
      5
┌────┐
│5   │
└Int8┘
~~~

After pressing <Enter> the session responds with the result of the expression and  positions the
cursor on the next line, indented 6 spaces, ready for you to enter another expression. Expressions
can include functions:

~~~
      sum 1 2 3 4 5
┌────┐
│15  │
└Int8┘
~~~

The result of an expression may be assigned to a name for further use. The assignment operator is
the equal symbol:

~~~
      A=1 2 3
~~~

Assignment suppresses the display of the result in the session. However, the assigned name is now a
variable that may be referenced in any expression, and it may be displayed by simply entering it:

~~~
      A
┌────┐
↓1   │
│2   │
│3   │
└Int8┘
~~~

The session provides access to the FlipDB database server via the
→[##.##.Reference.Objects.Scripting] object. Simply type "Scripting" into the session to access
the scripting object. The →[##.##.Reference.Objects.Scripting.Properties.Server] property of the
scripting object returns the one and only instance of the Server object available in a given
session. With an instance of the server in hand, its properties and methods may be exercised.

The Server has a method for creating the long -standing and well-known suppliers and parts database
of E.F. Codd and C.J Date, who use it in their many writings. However, before creating it, it must
be deleted if it already exists. This is done with the
→[##.##.Reference.Objects.Server.MethodList.DeleteDatabase] method:

~~~
      Scripting.Server.DeleteDatabase 'SandP'
┌───────┐
│0      │
└Boolean┘
~~~

Now that it is sure not to exist, it may be created:

~~~
      D=Scripting.Server.BuildSuppliersAndParts ''
~~~

The result is a →[##.##.Reference.Objects.Database] object, which in turn exposes properties and
methods. For example, the →[##.##.Reference.Objects.Database.MethodList.GetTable] method returns
a table object:

~~~
      D.GetTable 'S'
── SandP.S ──────────────────────────────────────
 ┌SNO────┐  ┌SNAME─────┐  ┌STATUS┐  ┌CITY──────┐
 ↓S1     │  ↓Smith     │  ↓20    │  ↓London    │
 │S2     │  │Jones     │  │10    │  │Paris     │
 │S3     │  │Blake     │  │30    │  │Paris     │
 │S4     │  │Clark     │  │20    │  │London    │
 │S5     │  │Adams     │  │30    │  │Athens    │
 └Char(2)┘  └Char(10)──┘  └Int8──┘  └Char(10)──┘
── 5 rows by 4 columns ──────────────────────────
~~~

## Playing Help Topics and Scripts

Help topics (including this one) and scripts may be "played" in the session by selecting the
File/Play menu from the Help window and the script editor window respectively. When a help topic
is played, the executable code from the topic is loaded into the session, and may be played back
interactively, line-by-line, by pressing the <F8> key. This allows you to easily follow the help
topic in the session, without typing all the input lines, where you can inspect and explore the
objects and variables. Similarly, when a script is played, it is loaded into the session for
line-by-line playback. Note that a line of a script or help topics that contain a control
structure will not play back in the session as it is not a complete statement.

