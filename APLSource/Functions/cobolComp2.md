# cobolComp2

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=cobolComp2 X
~~~

X must be Char and is interpreted as a COBOL COMP-2 field. R is X converted to Float.

This function is normally only used when importing COBOL files in EBCDIC character sets. In COBOL
terminology, COMP-2 is short for COMPUTATIONAL-2, and is also sometimes referred to as Floating
Point. It is 8 bytes long.

