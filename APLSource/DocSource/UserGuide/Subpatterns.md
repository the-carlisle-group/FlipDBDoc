# Subpatterns and BackReferences

Parentheses are used to create subpatterns
which have a number of overlapping uses.

The first is to localize a set of alternatives:

~~~
      regexMatch 'the White Sox and the Red Sox' 'the (Red|White) Sox'
┌─────────────┐
↓the White Sox│
│the Red Sox  │
└Char(13)─────┘

~~~

The second is to group a set of characters for repetition:

~~~
      regexMatch '123abcabcabc456' '3(abc)+'
┌──────────┐
↓3abcabcabc│
└Char(10)──┘
~~~

The third is to capture the match for future use in the
search pattern or in the replace pattern. By default,
subpatterns are captured and numbered in order from 1 to n.
They can then be referenced by number using 
 or more formally g{n}.
This is known as a *backreference*.

Consider the problem of finding consecutive duplicate words.
The backreference allows us to look for something we previously found:

~~~
      regexMatch 'find the the double double words' '(\w+)\s+\1'
┌─────────────┐
↓the the      │
│double double│
└Char(13)─────┘

~~~

Backrefernces may also be used in the replace pattern:

~~~
       regexReplace 'find the the double double words' '(\w+)\s+\1' '\1'
┌─────────────────────┐
│find the double words│
└Char(21)─────────────┘
~~~

