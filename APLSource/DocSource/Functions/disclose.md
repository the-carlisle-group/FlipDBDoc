# disclose

Type:{.prefix}

Structural{.info}

Categories:{.prefix}

Misc{.info}

Simplify column structure.{.purpose}

~~~
R=disclose X
~~~

X may be any column. If X is enclosed, R is simple. If X is scalar or simple the function has no
effect. If X is partitioned, R is an array of simple columns created from each item of X.

See also: →[enclose], →[enlist]

## Examples

~~~
       disclose 5
┌────┐
│5   │
└Int8┘
       enclose 1 2 3
┌───────┐
│[1,2,3]│
└Int8───┘
       disclose enclose 1 2 3
┌────┐
↓1   │
│2   │
│3   │
└Int8┘
       enclose (1 2 3) (4 5 6 7) (8 9)
┌─────────┐
↓[1,2,3]  │
│[4,5,6,7]│
│[8,9]    │
└Int8─────┘
      disclose enclose (1 2 3) (4 5 6 7) (8 9)
 ┌────┐  ┌────┐  ┌────┐
 ↓1   │  ↓4   │  ↓8   │
 │2   │  │5   │  │9   │
 │3   │  │6   │  └Int8┘
 └Int8┘  │7   │
         └Int8┘
~~~

