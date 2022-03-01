# reverse

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

~~~
R=reverse Y
~~~

Y is any column or object array. R is Y with its elements, partitions or objects in reverse order.

## Examples

~~~
      reverse 1 2 3 4
┌────┐
↓4   │
│3   │
│2   │
│1   │
└Int8┘
      reverse 'One,Two,Three'
┌───────┐
↓Three  │
│Two    │
│One    │
└Char(5)┘
      a=1 2 3
      b='A,C,B,C'
      c=2
      reverse a b c
 ┌────┐  ┌───────┐  ┌────┐
 │2   │  ↓A      │  ↓1   │
 └Int8┘  │C      │  │2   │
         │B      │  │3   │
         │C      │  └Int8┘
         └Char(1)┘
~~~

