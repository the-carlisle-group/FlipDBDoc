# cobolComp1

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=cobolComp1 X
~~~

X must be Char and is interpreted as a COBOL COMP-1 field. R is X converted to Float.

This function is normally only used when importing COBOL files in EBCDIC character sets. In COBOL
terminology, COMP-1 is short for COMPUTATIONAL-1, and is also sometimes referred to as Floating
Point. It is 4 bytes long.

## Examples

~~~
     cobolComp1 hexToChar '3F800000'
┌───────┐
│0.03125│
└Float──┘
~~~

