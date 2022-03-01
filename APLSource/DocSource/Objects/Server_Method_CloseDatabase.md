# CloseDatabase Method

Applies to:{.prefix}

→[##.##.Server]{.info}

The CloseDatabase method changes the state of a database from Open to Closed.

The argument is composed of 1 item:

|-|-|
|1|Database name|String|

The result is composed of 1 item:

|-|-|
|1|Return code|0|

For example:

~~~
      S.CloseDatabase 'SANDP'
~~~

