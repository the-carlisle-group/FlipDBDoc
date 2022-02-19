# Assertions

An assertion is a test on the characters before or after the current
character. We have already seen the assertion ^ for matching the start of the
string and $ for matching the end of the string. Another common assertion
is \w, for a word boundary:

~~~
     regexMatch 'find all the words' '\w+'
┌───────┐
↓find   │
│all    │
│the    │
│words  │
└Char(5)┘

~~~

There can be multiple assertions:

~~~
      regexMatch 'find the last word' '\w+$'
┌───────┐
↓word   │
└Char(4)┘
~~~

A *lookahead* assertions looks for a subpattern past the current position.
They can be *positive* or *negative*.
A postive lookahead asseration begins with open parenthesis and an equals sign.
For example, to find the notes that are sharped in the D major scale:

~~~
     regexMatch 'D E F# G A B C# D' '[ABCDEFG](?=#)'
┌───────┐
↓F      │
│C      │
└Char(1)┘
~~~

Note that sharps, the assertion part, are not returned as part of the match.
This is a *positive* assertion.  A *negative* lookahead assertion
begins uses the exclamation point rather than the equals sign:

~~~

      regexMatch 'D E F# G A B C# D' '[ABCDEFG](?!#)'
┌───────┐
↓D      │
│E      │
│G      │
│A      │
│B      │
│D      │
└Char(1)┘
~~~

Lookbehind asserations behave in an analagous manner.
A positive lookbehind assertion begins with (?<= while
a negative lookbehind assertion begins with (?<!.



