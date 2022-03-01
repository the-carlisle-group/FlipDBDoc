# reverseChar

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Reverses the characters in a string.{.purpose}

~~~
R=reverse X
~~~

X is Char. R is X with the characters in reverse order.

## Examples

~~~
      reverseChar  'Paul'
┌───────┐
│luaP   │
└Char(4)┘
      a='Paul,Stephen,Joe'
      a
┌───────┐
↓Paul   │
│Stephen│
│Joe    │
└Char(7)┘
      reverseChar a
┌───────┐
↓luaP   │
│nehpetS│
│eoJ    │
└Char(7)┘
~~~

