# roll

Type:{.prefix}

Scalar{.info}

Categories:{.prefix}

Arithmetic{.info}

~~~
R=roll X
~~~
X must be non-negative integer or boolean.

For an element of X equal to N and greater than 0, the corresponding element of R
is a (pseudo) random number selected from the first N natural numbers (0 to N-1).
If N is 0, R is a random number between 0 and 1.

The function is named by analogy with the rolling of a die.

See also →[deal]

## Examples

~~~
      roll 6
┌────┐
│4   │
└Int8┘
      roll 6 6
┌────┐
↓5   │
│1   │
└Int8┘
      roll 50 100 1000
┌─────┐
↓21   │
│76   │
│477  │
└Int16┘
      roll 2 2 2 2
┌────┐
↓0   │
│1   │
│1   │
│0   │
└Int8┘
      roll 0 0 0
┌────────────┐
↓0.1922756546│
│0.4547900874│
│0.7254891488│
└Float───────┘
~~~

