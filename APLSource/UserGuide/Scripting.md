# Scripting

A script is a program, or an ordered set of FlipDB expressions. Scripts are created and edited from
the FlipDB explorer, and may be organized into folders. When editing a script, it may be executed
from the main menu of the editor, by selecting the Run button. In addition, a script may be traced
line-by-line, inspecting the results along the way, using the Trace button.

Scripts may call scripts using the →[*.executeScript] function.
This function may also be used to execute scripts in queries or
in the →[*.Globals] property of a report, or interactively in the session.

A script may take an argument and return a result. In general, if a script takes an argument or returns
a result, it is designed to be called by another script or run interactively in the session. The explicit
result of a script that is run from the main menu is discarded.

A script may access its argument using the reserved word `Argument`. The argument may be any FlipDB object
from a →[*.DataColumn], to a →[*.DataTable], to a →[*.PropertySpace]. If no argument is provided to the
script, the key word `Argument` returns a zero within the script.

The result of a script is specified using the key word `Result`, which may be set to any FlipDB object.
If not specified, the result of the script defaults to zero.

A scripts access the server via the reserved word `TheServer`.





