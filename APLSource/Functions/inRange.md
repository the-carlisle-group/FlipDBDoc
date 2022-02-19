# inRrange

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Relational{.info}

~~~
R=X inRange Y Z
~~~

X must be numeric or temporal. Y and Z are scalar items of a type compatible with X. B is Boolean,
with a 1 if X is in the range bounded by Y and Z. The lower bound is closed, and the upper bound
is open. Thus this function is equivalent to:

~~~
        (X >= Y) and (X &lt; Z)
~~~

