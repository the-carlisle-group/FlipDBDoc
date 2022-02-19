# Delete Method

Applies to:{.prefix}

→[##.##.Server]{.info}

The Delete method deletes a resource from the path portion of its URL. The schema and domain should
not be included in the argument.

The argument is composed of 1 item:

|-|-|
|1|Name|String|

The result is composed of 1 item:

|-|-|
|1|ReturnCode|Boolean|

For Example:

~~~
       s.Exists '/Databases/SandP'
┌───────┐
│1      │
└Boolean┘
       s.Delete '/Databases/SandP'
┌───────┐
│0      │
└Boolean┘
       s.Exists '/Databases/SandP'
┌───────┐
│0      │
└Boolean┘
~~~

