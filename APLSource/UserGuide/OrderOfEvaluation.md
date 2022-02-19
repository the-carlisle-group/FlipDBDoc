# Order of Evaluation

There is no hierarchy of functions. There is one, single, uniform order of operations: right to
left. Parentheses are used to override the order of evaluation. Consider the following expression:

~~~
     2 * 3 + 4 / 5
┌─────┐
│7.6  │
└Float┘
~~~

One way of reading this is: 2 times everything to the right which is 3 plus everything to the
right, which is 4 divided by 5. We can place parentheses to this effect, and see how they are
redundant and have no effect:

~~~
     2 * (3 + (4 / 5))
┌─────┐
│7.6  │
└Float┘
~~~

The order can be changed by placing appropriate parentheses:

~~~
     ((2 * 3) + 4) / 5
┌─────┐
│2    │
└Float┘
~~~

