# Quick Reference

## Character Classes

|Class       |Meaning
|------------|-------
|`.`           |Any character
|`\d`          |A decimal digit, 0-9
|`\D`          |Not a decimal digit
|`\s`          |A whitespace character
|`\S`          |Not a whitespace character
|`\w`          |A "word" character
|`\W`          |Not a "word" character
|`[...]`       |Any character in list
|`[^...]`      |Any character not in list

## Assertions

|Assertion   |Meaning
|------------|-------
|`^`           | Start of string
|`$`           | End of string
|`\b`          | Word boundary
|`\B`          | Not a word boundary
|`(?=...)`     | Positive lookahead
|`(?!...)`     | Negative lookahead
|`(?<=...)`    | Positive lookbehind
|`(?<!...)`    | Negative lookbehind

## Quantifiers

|Quantifier  |Meaning
|------------|:-------
|`?`           |0 or 1, greedy
|`?+`          |0 or 1, possesive
|`??`          |0 or 1, lazy
|`*`           |0 or more, greedy
|`*+`          |0 or more, possesive
|`*?`          |0 or more, lazy
|`+`           |1 or more, greedy
|`++`          |1 or more, possesive
|`+?`          |1 or more, lazy
|`{n}`         |exactly n
|`{n,m}`       |at least n, no more than m, greedy
|`{n,m}+`      |at least n, no more than m, possesive
|`{n,m}?`      |at least n, no more than m, lazy
|`{n,}`        |n or more, greedy
|`{n,}+`       |n or more, possesive
|`{n,}?`       |n or more, lazy

## Grouping and referencing

|Sequence    |Meaning
|------------|-------
|`(...)`       | Group and capture
|`(?<name>...)`| named group
|`(?:...)`     | Group but don't capture
|`
`          | reference by number (shortcut)
|`g{n}`       | reference by number
|`l<name>`    | reference by name

## Other
|Sequence    |Meaning
|-------------|-------
|`...|...|...`| Alternation
|`(?i)         `| Ignore case, caseless matching
|``              | Escape
|`Q...E`        | Quote

