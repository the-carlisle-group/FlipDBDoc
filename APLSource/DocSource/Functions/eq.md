# eq

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Relational{.info}

Equals; equality; equal to{.purpose}

~~~
R=X eq Y
~~~

X and Y must be conforming types (i.e., both numeric, both Char, etc.) R is boolean being 1 where X
equals Y, and 0 otherwise. eq may also be written as '`==`'.

See also: →[match]

## Examples

~~~
      1 eq 0 -2 1 3.4
┌───────┐
↓0      │
│0      │
│1      │
│0      │
└Boolean┘
       '2009-02-28,2009-02-29' eq '2009-02-29'
┌───────┐
↓0      │
│1      │
└Boolean┘
      'Mike,Tim,Tim' == 'Tim,Jon,Tim'
┌───────┐
↓0      │
│0      │
│1      │
└Boolean┘
~~~

