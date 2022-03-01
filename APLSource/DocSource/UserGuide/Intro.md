# Introduction

A *regular expression* ("regex", with a hard g, for short) is a string containing normal characters
and *metacharacters* or *metasequences* that defines a pattern for
seaching, extracting, and replacing text in a target string.

FlipDB provides a family of 6 functions for working with regular expressions:
→[*.regexSearch], →[*.regexMatch], →[*.regexIndex], →[*.regexLength], →[*.regexReplace], and →[*.regexSplit].

What follows is an introduction to regular expressions, specifically from the the point of
view of searching and replacing database columns, and the family of FlipDB regex functions.
Regular expressions are used for working with documents and text of any kind and are
embedded in many programming languages, text editors, and other programs.
Regular epressions have many features and options not mentioned here.

FlipDB uses the open source libary PCRE (Perl Compatible Regular Expressions).

## The Basics
The regexSearch function takes a target string (usually a data or database column) and regex
as its argument, and returns a boolean if the search pattern is found.  This function
is most often used in a where clause of a query. Let's look at a column with 3 US states:

~~~
      State='Mississippi,Missouri,Minnesota'
~~~

In a regex, a character matches itself, so to look for the string 'iss', we simply use the trivial regex 'iss'

~~~

      State   (regexSearch State 'iss')
 ┌───────────┐  ┌───────┐
 ↓Mississippi│  ↓1      │
 │Missouri   │  │1      │
 │Minnesota  │  │0      │
 └Char(11)───┘  └Boolean┘
 ~~~

The power of regular expressions however comes from metacharacters and metasequences.
For example, the dot metacharacter matches any character.
Once we add metacharacters or sequences to the regex, text that matches the pattern
will not always be the same (or even the same length).
The regexMatch function returns that actual matching
text, for every match (by default), rather than just a boolean indicating the existence of a match:

~~~
      State   (regexMatch State 'i.')
 ┌───────────┐  ┌──────────┐
 ↓Mississippi│  ↓[is,is,ip]│
 │Missouri   │  │[is]      │
 │Minnesota  │  │[in]      │
 └Char(11)───┘  └Char(2)───┘

~~~

Note that there are multiple matches in the case of Mississippi. The regexIndex
function returns the starting position of each match:

~~~
      State   (regexIndex State 'i.')
 ┌───────────┐  ┌───────┐
 ↓Mississippi│  ↓[1,4,7]│
 │Missouri   │  │[1]    │
 │Minnesota  │  │[1]    │
 └Char(11)───┘  └Int8───┘
~~~

Now consider the payment history column, recording the number
of times 30 days delinquent over the last 12 months:

~~~
      PayHist='000000000000,333012333333,0001234444FF'
      PayHist
┌────────────┐
↓000000000000│
│333012333333│
│0001234444FF│
└Char(12)────┘
~~~

The + metacharacter is a *quantifier* and is used for
finding repetitions. It specifies one or more of the preceeding
character. Thus to find runs of 90 day delinquencies:

~~~
      PayHist (regexMatch  PayHist '3+')
 ┌────────────┐  ┌────────────┐
 ↓000000000000│  ↓[]          │
 │333012333333│  │[333,333333]│
 │0001234444FF│  │[3]         │
 └Char(12)────┘  └Char(6)─────┘
~~~

The regexLength function returns the length of each match:

~~~
   PayHist (regexLength  PayHist '3+')
 ┌────────────┐  ┌─────┐
 ↓000000000000│  ↓[]   │
 │333012333333│  │[3,6]│
 │0001234444FF│  │[1]  │
 └Char(12)────┘  └Int8─┘
~~~

Thus the maximum number of consecutive months 90 days delinquent is:

~~~
      0 high max regexLength  PayHist '3+'
┌─────┐
↓0    │
│6    │
│1    │
└Float┘
~~~

Sometimes we want to match not a specific character,
but a position in the subject string, for example the start
or the end of the string. This is called an *assertion*. The ^ metacharacter
matches the beggining of the string:

~~~
      State (1 regexMatch State '^.iss')
 ┌───────────┐  ┌───────┐
 ↓Mississippi│  ↓Miss   │
 │Missouri   │  │Miss   │
 │Minnesota  │  │       │
 └Char(11)───┘  └Char(4)┘

~~~

While $ matches the end of the string:

~~~
      State (1 regexMatch State '..i$')
 ┌───────────┐  ┌───────┐
 ↓Mississippi│  ↓ppi    │
 │Missouri   │  │uri    │
 │Minnesota  │  │       │
 └Char(11)───┘  └Char(3)┘
~~~

Paretheses may be used to create *subpatterns*.
Quantifiers then apply to the entire subpattern, not just a
single preceeding character:

~~~
      State (regexMatch State '(iss)+')
 ┌───────────┐  ┌────────┐
 ↓Mississippi│  ↓[ississ]│
 │Missouri   │  │[iss]   │
 │Minnesota  │  │[]      │
 └Char(11)───┘  └Char(6)─┘

~~~

## Match Limit
A search pattern may match a subject string 0 or more times.
Every regex function takes an optional left argument to specify the number of matches desired.
(Every function except regexSearch, where it is not applicable.)
The default value is 0, which indicates no limit, or match as many times as possible:


~~~
      State (regexMatch State 'i.')
 ┌───────────┐  ┌──────────┐
 ↓Mississippi│  ↓[is,is,ip]│
 │Missouri   │  │[is]      │
 │Minnesota  │  │[in]      │
 └Char(11)───┘  └Char(2)───┘
~~~

A positive integer N indicates the first N matches:

~~~
      State (2 regexMatch State 'i.')
 ┌───────────┐  ┌───────┐
 ↓Mississippi│  ↓[is,is]│
 │Missouri   │  │[is]   │
 │Minnesota  │  │[in]   │
 └Char(11)───┘  └Char(2)┘

~~~

A negative N indicates exactly the Nth match:

~~~
      State (-3 regexMatch State 'i.')
 ┌───────────┐  ┌───────┐
 ↓Mississippi│  ↓ip     │
 │Missouri   │  │       │
 │Minnesota  │  │       │
 └Char(11)───┘  └Char(2)┘

~~~

Setting the match limit to 1 or a negative value always yields a simple result,
rather than a partitioned result.

## Replace
The regexReplace function replaces each match with a replacement patttern.

~~~
      State (regexReplace State 'i.' 'X')
 ┌───────────┐  ┌────────┐
 ↓Mississippi│  ↓MXsXsXpi│
 │Missouri   │  │MXsouri │
 │Minnesota  │  │MXnesota│
 └Char(11)───┘  └Char(8)─┘
~~~

Replacement patterns are not limited to literal strings, and accept
various metacharacters. The & represents the match itself, so to insert
angle brackets around each match we may write:

~~~
      State (regexReplace State 'i.' '<&>')
 ┌───────────┐  ┌─────────────────┐
 ↓Mississippi│  ↓M<is>s<is>s<ip>pi│
 │Missouri   │  │M<is>souri       │
 │Minnesota  │  │M<in>nesota      │
 └Char(11)───┘  └Char(17)─────────┘
~~~

Replacing with an empty replacement pattern is way to extract the text
that does not match:

~~~
      State (regexReplace State 'i.' '')
 ┌───────────┐  ┌───────┐
 ↓Mississippi│  ↓Msspi  │
 │Missouri   │  │Msouri │
 │Minnesota  │  │Mnesota│
 └Char(11)───┘  └Char(7)┘
~~~

This is sort of an inverse to regexMatch which extracts
the text that does match.

## When NOT to use Regular Expressions

Regular expressions are extremely powerful, but the power comes
at a cost of execution speed.

Do not use regular expressions when there is a perfectly good
FlipDB function that does the job directly. The FlipDB text
functions will always be faster and more efficient than the
corresponding regex.

For example, if you are trimming the leading
zeros from a string, use the →[*.trimLeading] function. If, however,
you need to trim the leading zeros from a string only if the first
character after the leading zeros is not, say, the digit 9, by all means
use a regex function.

