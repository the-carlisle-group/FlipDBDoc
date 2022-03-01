# toZipCode

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=toZipCode X
~~~

X may be a numeric or character column. R is a character column of length 5.

This function makes no attempt to validate a zip code. It only formats the
input to look like a zip code. If any data is lost by converting to a character
column of width 5, then the result is filled with asterisks: `*****`.

## Examples:

~~~
      toZipCode '33040,910,Hello,New York'
┌───────┐
↓33040  │
│00910  │
│Hello  │
│*****  │
└Char(5)┘

      toZipCode 33040 1 2097 3 59718 6568834
┌───────┐
↓33040  │
│00001  │
│02097  │
│00003  │
│59718  │
│*****  │
└Char(5)┘
~~~
