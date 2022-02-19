# rotateChar

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Rotates the characters in a string.{.purpose}

~~~
R=X rotateChar Y
~~~

Y is Char. X is numeric and must be scalar or conform in length to Y. R is Y with the characters
rotated according to X.

## Examples

~~~
      2 rotateChar 'Paul'
┌───────┐
│ulPa   │
└Char(4)┘
       a='Paul,Stephen,Phil,Sam'
       a
┌───────┐
↓Paul   │
│Stephen│
│Phil   │
│Sam    │
└Char(7)┘
       2 rotateChar a
┌───────┐
↓ulPa   │
│ephenSt│
│ilPh   │
│mSa    │
└Char(7)┘
       1 2 3 0 rotateChar a
┌───────┐
↓aulP   │
│ephenSt│
│lPhi   │
│Sam    │
└Char(7)┘
~~~

