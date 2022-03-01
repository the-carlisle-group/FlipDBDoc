# circular

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=X circular Y
~~~

X and Y must both be numeric. X must be an integer in the range ¯7 le X le 7. R is numeric.

X determines which of a family of trigonometric functions to apply to Y, from the following table:

|`Range`|`Domain`|`(-X) ○ Y`|`X`|`X ○ Y`|`Range`|
|-|-|-|-|-|-|
|`0≤R≤1`|`(-○.5)<R≤○.5`|`0≤R≤○1`|`(|R)≤○.5`|`R≥○1`||`R≥○1`||
|`(|Y)≤1`|`(|Y)≤1`|`(|Y)≤1`||`(|Y)≥1`||`Y≥1`|`(|Y),1`|
|`(1-Y*2)*.5`|`Arcsin Y`|`Arccos Y`|`Arctan Y`|`(¯1+Y*2)*.5`|`Arcsinh Y`|`Arccosh Y`|`Arctanh Y`|
|`0`|`1`|`2`|`3`|`4`|`5`|`6`|`7`|
|`(1-Y*2)*.5`|`Sine Y`|`Cosine Y`|`Tangent Y`|`(1+Y*2)*.5`|`Sinh Y`|`Cosh Y`|`Tanh Y`|
|`0≤R≤1`|`(|R)≤1`|`(|R)≤1`||||`R>1`|`(|R)<1`|

## Examples

~~~
      0 1 2 3 4 5 6 7 circular 1
┌────────────┐
↓0           │
│0.8414709848│
│0.5403023059│
│1.557407725 │
│1.414213562 │
│1.175201194 │
│1.543080635 │
│0.761594156 │
└Float───────┘
~~~

