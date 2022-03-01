# Repetition and Quantifiers

Repetition is specified using a *quantifier*.  The most general form for a quantifier
explicitly specifies the minimum and maximum number of repetitions,
separated by a comma, enclosed in braces:

~~~
{n,m}
~~~

To match 2 to 4 consecutive occurances of the character b:

~~~
      regexMatch 'bbbbababbcbbb' 'b{2,4}'
┌───────┐
↓bbbb   │
│bb     │
│bbb    │
└Char(4)┘

~~~

Note the backslash here is a FlipDB escape for the comma, and NOT a regex
metacharacter. Literal arguments must first be processed by FlipDB
and the comma would indicate a string separator if not escaped.

For no limit to the number of repetitions, leave off the maximum using {n,}:

~~~
      regexMatch 'The beautiful giant sequoias.' '[aeiou]{2,}'
┌───────┐
↓eau    │
│ia     │
│uoia   │
└Char(4)┘
~~~

For exactly n repetitions, use {n} with no comma:

~~~
      regexMatch 'The beautiful giant sequoias.' '[aeiou]{3}'
┌───────┐
↓eau    │
│uoi    │
└Char(3)┘
~~~

Regex provides shortcuts for the 3 most commonly used quantifiers:

Shortcut|Quantifier|Meaning
*       |{0,}      |match 0 or more times
+       |{1,}      |match 1 or more times
?       |{0,1}     |match 0 or 1 time

## Greedy versus Non-Greedy Quantifiers

Quantifiers attempt to match as much as possible up to the maximum specified.
This is called "greediness". Sometimes this behavior is surprising.
Consider looking for the character a, followed by an unlimited number of any
other character, followed by an e:

~~~
      regexMatch  'abcdefghijke'   'a.+e'
┌────────────┐
↓abcdefghijke│
└Char(12)────┘

~~~

You might expect the match to be just "abcde", but the quantifier + is greedy
and it seeks the largest match possible, and thus matches the whole string.
A quantifier can be specified to be non-greedy (also known as lazy)
A lazy quantifier matches the fewest number of repetitions.
This is done by following the quantifier with a ?:

~~~
   regexMatch  'abcdefghijke'   'a.+?e'
┌───────┐
↓abcde  │
└Char(5)┘
~~~

Matching HTML tags is a classic example of greedy versus non-greedy behavior:

~~~
      regexMatch  'To <em>be</em>or <em>not</em> to be'   '<em>.+</em>'
┌──────────────────────────┐
↓<em>be</em>or <em>not</em>│
└Char(26)──────────────────┘

      regexMatch  'To <em>be</em>or <em>not</em> to be'   '<em>.+?</em>'
┌────────────┐
↓<em>be</em> │
│<em>not</em>│
└Char(12)────┘
~~~

Note well the difference.

