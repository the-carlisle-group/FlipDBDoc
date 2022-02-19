# cobolChar

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=cobolChar X
~~~

X must be Char. R is X interpreted as EBCDIC and converted to ASCII.

This function is normally only used when importing COBOL files in EBCDIC character sets.

## Examples

~~~
       'ÑÖÈÕ@,ÑÁÔÅâ'
┌───────┐
↓ÑÖÈÕ   │
│ÑÁÔÅâ  │
└Char(5)┘
       cobolChar 'ÑÖÈÕ@,ÑÁÔÅâ'
┌───────┐
↓JOHN   │
│JAMES  │
└Char(5)┘
~~~

