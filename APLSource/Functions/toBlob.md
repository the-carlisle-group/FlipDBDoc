# toBlob

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Casting{.info}

~~~
R=toBlob X
~~~

X is any column. R is Blob with the same value and structure as X unless X is Char when items of R
are without trailing blanks.

## Examples

~~~
      toBlob 'Hello world'
┌──────┐
│[blob]│
└Blob──┘
~~~

