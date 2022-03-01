# OrderBy Property

Applies to:{.prefix}

→[##.##.Query]{.info}

The OrderBy property specifies a sort order to be applied to the result of the query. You can sort
by a single column:

~~~
      Q.OrderBy='WEIGHT' 'Down'
~~~

Or multiple columns and multiple directions:

~~~
      Q.OrderBy='COLOR,WEIGHT' 'Up,Down'
~~~

