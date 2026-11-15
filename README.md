# C-- Interpreter in Racket

An interpreter for **C--**, a small C-like teaching language, written in Racket. It reads a C-- program from a file, turns it into tokens, parses them into an abstract syntax tree, and runs the program.

Written for the Functional Programming module of my BSc Computer Science at Birkbeck, University of London (2020).

## What it does

The interpreter works in three stages:

1. **Lexing.** A lexer built with Racket's `parser-tools/lex` turns the source text into tokens: numbers, variable names, keywords (`if`, `else`, `while`, `print`), operators and brackets.
2. **Parsing.** An LALR parser built with `parser-tools/yacc` turns the tokens into an abstract syntax tree, represented as nested Racket lists. Operator precedence and associativity are declared in the grammar, so `m - 3 + n / 2 * 4` is grouped the way C would group it.
3. **Evaluation.** A recursive evaluator walks the tree, keeps variables in a hash table, and carries out each statement in order.

## The C-- language

C-- supports:

- **Integer variables** and assignment: `x = 5`
- **Arithmetic:** `+`, `-`, `*`, `/` (integer division) and `%`
- **Comparisons:** `==`, `!=`, `<`, `<=`, `>`, `>=`
- **Logical operators:** `&&` and `||`
- **Conditionals:** `if (...) ... else ...`, with or without `{ }` blocks
- **Loops:** `while (...) { ... }`
- **Output:** `print` with one or more comma-separated values

The full language definition is in [`TheC--Language.pdf`](TheC--Language.pdf).

## Example

This program ([`cmmExamples/example3.cmm`](cmmExamples/example3.cmm)) prints the factorials from 1 to 8:

```
n = 8
k = 1
f = 1
while (k <= n) {
    f = f * k
    print k,f
    k = k + 1
}
```

Output:

```
1 1
2 2
3 6
4 24
5 120
6 720
7 5040
8 40320
```

More example programs, with their expected output, are in [`cmmExamples/`](cmmExamples/).

## How to run it

You need [Racket](https://racket-lang.org/), which includes the DrRacket editor.

**In DrRacket:** open `project1.rkt`, click **Run**, then type at the `>` prompt:

```racket
(interpret "cmmExamples/example3.cmm")
```

**From a terminal**, in the repository folder:

```bash
racket -e '(require (file "project1.rkt")) (interpret "cmmExamples/example3.cmm")'
```

Use the path to any `.cmm` file to run your own C-- programs.

## Files

```
project1.rkt         the interpreter: lexer, parser and evaluator
cmmExamples/         example C-- programs (.cmm) and their expected output (.output)
TheC--Language.pdf   the C-- language definition, provided with the coursework
```

## Limitations and possible improvements


* Error messages don’t give a location. A syntax error prints “Couldn’t parse” with the offending token, but no line number.
* Division by zero stops with Racket’s own error message rather than a C-- one.


## Credits

The coursework brief and the C-- language definition were provided by Birkbeck. The lexer and parser follow the structure of the `calc.rkt` example in Racket's `parser-tools` library.
