# identical

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Logical{.info}

Identical; all items identical{.purpose}

~~~
R=identical X
~~~

X may be of any class.  R is boolean; 1 if all items of X are identical; 0 if not.

## Examples

~~~
        identical 1 1 1 1
┌───────┐
│1      │
└Boolean┘
        identical 'A,B,A'
┌───────┐
│0      │
└Boolean┘
       identical 5
┌───────┐
│1      │
└Boolean┘
~~~

