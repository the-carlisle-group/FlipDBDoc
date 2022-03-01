# Exists Method

Applies to:{.prefix}

→[##.##.Server]{.info}

The Exists method verifies the existance of a resource from the path portion of its URL. The schema
and domain should not be included in the argument.

The argument is composed of 1 item:

|-|-|
|1|Name|String|

The result is composed of 1 item:

|-|-|
|1|Exists|Boolean|

~~~
      s=Scripting.Server
      s.Exists '/Databases/SandP'
┌───────┐
│1      │
└Boolean┘
      s.Exists '/Databases/SandP/NoSuchTable'
┌───────┐
│0      │
└Boolean┘
~~~

