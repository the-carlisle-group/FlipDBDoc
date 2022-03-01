# Character Classes

A character class is a list of characters. If a character is in the list,
it's a match. A character class matches (or not) exactly one character.
In general, character classes are delimited by brackets.
For example, to search for any three character followed by a vowel we can write:

~~~
      State ( regexMatch State '...[aeiou]')
 ┌───────────┐  ┌───────────┐
 ↓Mississippi│  ↓[issi,ippi]│
 │Missouri   │  │[isso]     │
 │Minnisota  │  │[inni,sota]│
 └Char(11)───┘  └Char(4)────┘
~~~

A character class can be specified as everything that is NOT listed by
starting it off with a ^. So to find 4 character strings where the last
character is NOT a vowel, we can write:

~~~

      State ( regexMatch State '...[^aeiou]')
 ┌───────────┐  ┌───────────┐
 ↓Mississippi│  ↓[Miss,ssip]│
 │Missouri   │  │[Miss]     │
 │Minnisota  │  │[Minn,isot]│
 └Char(11)───┘  └Char(4)────┘

~~~

In addition to the general syntax for character class using brackets,
there are various shortcuts for specific character classes.
(The dot itself is the most general character class; it matches every character.)
For example \d matches any digit, 0 to 9, while \D matches any character
that is not a digit:

~~~
      PayHist (regexMatch PayHist '\d+')
 ┌────────────┐  ┌──────────────┐
 ↓000000000000│  ↓[000000000000]│
 │333012333333│  │[333012333333]│
 │0001234444FF│  │[0001234444]  │
 └Char(12)────┘  └Char(12)──────┘

      PayHist (regexMatch PayHist '\D+')
 ┌────────────┐  ┌───────┐
 ↓000000000000│  ↓[]     │
 │333012333333│  │[]     │
 │0001234444FF│  │[FF]   │
 └Char(12)────┘  └Char(2)┘
~~~

These shortcuts can be used inside brackets as well. For example,
to search for runs of the digits 3 and 4, and any non-digit, we can write:

~~~
      PayHist (regexMatch PayHist '[34\D]+')
 ┌────────────┐  ┌────────────┐
 ↓000000000000│  ↓[]          │
 │333012333333│  │[333,333333]│
 │0001234444FF│  │[34444FF]   │
 └Char(12)────┘  └Char(7)─────┘
~~~

Note that metacharacters like + or * (or dot) are treated as literals when listed
inside class brackets. Here the dot is used once inside the character class as a simple dot,
and once outside the class as a metacharacter indicating any character:

~~~
      regexMatch 'One.Two,Three*Four' '[.*].+'
┌───────┐
↓[.Two] │
│[*Four]│
└Char(5)┘
~~~

