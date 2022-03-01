# toChar

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=[Y] toChar X
~~~

X is any column but Blob. R is X formatted. Y is optional; If present it must be a Format string.

## Examples

~~~
      toChar 1 2 3
┌───────┐
↓1      │
│2      │
│3      │
└Char(1)┘
      'I8' toChar 1 2 3
┌────────┐
↓       1│
│       2│
│       3│
└Char(8)─┘
      'F10.4' toChar 1.2 0.009 -2
┌──────────┐
↓    1.2000│
│    0.0090│
│   -2.0000│
└Char(10)──┘
~~~

