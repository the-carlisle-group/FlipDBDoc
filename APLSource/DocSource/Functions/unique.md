# unique

Type:{.prefix}

Aggregate{.info}

Categories:{.prefix}

Logical{.info}

Unique; all items identical{.purpose}

~~~
R=unique X
~~~

X may be of any class.  R is boolean; 1 if all items of X are unique, i.e. no items are duplicated;
0 otherwise.

See also: →[distinct]

## Example

~~~
      unique 1 2 3 4 5
┌───────┐
│1      │
└Boolean┘
      unique 1 2 3 4 1
┌───────┐
│0      │
└Boolean┘
~~~

