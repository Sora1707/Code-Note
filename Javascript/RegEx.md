# RegEx cheat sheet

### Quantifiers (for preceeding value)

-   `*`: 0 or more times
-   `+`: at least 1 time
-   `{min, max}`: in range (min or max can be omitted)
-   `.`: any above
-   `?`: optional
-   `*/+/{}` + `?`: non-greedy (stops when finished)

### Character classes

-   `[xyz]`, `[a-d]`: any single character enclosed
-   `[^xyz]`, `[^a-c]`: any single character **NOT** enclosed (negated)
-   `.`: any single character except line terminators
-   `.`: inside character class, just a dot
-   `\d`: `[0-9]`
-   `\w`: `[A-Za-z0-9_]` (alphanumberic)
-   `\s`: `[\f\n\r\t\v\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]` (normal space)
-   `\UpperCase`: `^` + `\NormalCase`
-   `\t`: tab
-   `\n`: new line
-   `\r`: return
-   `\v`: vertical tab
-   `\f`: form feed
-   `[\b]`: backspace
-   `\0`: null
-   `x|y`: disjunction

### Assertion

-   `^`: match at the beginning (ex: `^\d{3}` placed at the beginning)
-   `$`: match at the end (ex: `-\d{3}$` placed at the end)
-   `\b`: word boundary (not followed or preceeded by another word character)

### Conditional

#### Look ahead

-   `x(?=y)`: match x if followed by y
-   `x(?!y)`: match x if **NOT** followed by y

#### Look behind

-   `(?<=y)x`: match x if preceeded by y
-   `(?<!y)x`: match x if **NOT** preceeded by y
