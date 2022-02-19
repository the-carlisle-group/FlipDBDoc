# takeChar

Applies to:{.prefix}

Character{.info}

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Text{.info}

Take some chars.{.purpose}

~~~
R=X takeChar Y
~~~

Y is Char.

X is a scalar Integer: +/- length

R is Char and of the same structure as Y

X characters are returned for each row of Y from the left if X is positive; leftwards from the last
non-blank if negative.

## Examples

~~~
     2 takeChar 'Paul,Phil'
┌───────┐
↓Pa     │
│Ph     │
└Char(2)┘
     2 takeChar enclose 'Paul,Phil'
┌───────┐
│[Pa,Ph]│
└Char(2)┘
      2 takeChar enclose 'Paul,Phil,Kai' 'Tim,Tom'
┌──────────┐
↓[Pa,Ph,Ka]│
│[Ti,To]   │
└Char(2)───┘
~~~

