# CML Format Details

This document describes the details of the Conditional Markup Language.


## Comments

Comments can either span a single or multiple lines. To comment a single line
you can use *//*. For multiple lines the text must be enclosed with /\* and
\*/.

Examples:

```
// This comment lasts only this line.

/*
  This comment can span multiple lines.
 */
```


## Indent

The CML format is indent sensitive. One indent level has to be 2 characters
wide.


## Primitives

### Strings

String primitives are enclosed with double quotes and can span multiple lines.
Whitespaces at the beginning or the end of a string are cut off. Spaces, tabs
and newlines in the middle of a string are replaced with a single space
character. To enfore these characters use the following escape sequences:

* ^n for a newline.
* ^t for a tab.
* ^s for a space.
* ^^ for a single ^.
* ^" for a ".

Examples:

```
"Hello World"
"
This string uses multiple lines but is evaluated as a single one.
"
"This string contains^nmultiple lines."
```

### Integer Numbers

Integer numbers can be written either with decimal or hexadecimal base. If the
number is prefixed with *0x* the following characters are interpreted as
hexadecimal number. Otherwise they are interpreted with a decimal base.

To increase the readability a number might contain the _ character, which is
completely ignored during evaluation.

Examples:

```
11
0x1A
-2_2
```

### Floating Point Numbers

Floating point numbers can be written in decimal form or scientific notation.

Examples:

```
1.1
-4.32e-2
```

### Boolean Values

Boolean primitives are *true* and *false* which are keywords of the format.


## Arrays

Items are combined in an array by prefixing the items with the *-* character
and aligning them with the same indent.
Only one item is allowed per line. But it is also possible to insert a blank
line between two array items for readability.

Hint: An empty array is expressed by a single *-* character without a value.

Example:

```
- 1
- 2

- 3
```


## Objects

Objects consist of multiple keys and their values. Keys with the same indent
belong to the same object. A key consists of the following characters and cannot
start with a number:

* a-z, A-Z
* 0-9
* \_, .

The key and value are separated by the *:* character. In case the value is a
primtive it has to start on the same line. If the value is an array or an
object it has to start on the next line.

Hint: If two keys have the same name their values are combined by the parser.
This will only work if the value is either an array or an object. If the value
holds an object the keys inside the object itself must be combineable.

Example:

```
student:
  first.name: "Klaus"
  last.name: "Rudolf"

teacher:
  first.name: "Peter"
  last.name: "Stumpf"
  students:
  - first.name: "Klaus"
    last.name: "Rudolf"
  - first.name: "Adam"
    last.name: "Riese"
```


## Conditions

Keys inside an object can be prefixed with a condition in a separate line. Only
if the condition is evaluated to *true* the key is included. Otherwise it is
ignored. Conditions are enclosed with *[* *]* and can cover multiple lines
without any restriction to indentation.

A condition expression consists of symbols, literals and operators. The symbols
are defined when parsing the file. In case a symbol is not defined and used
without the ? operator or an operator cannot be applied because of a type
mismatch the whole condition evaluates to *false*. Symbols can contain the same
characters as object keys.

The following operators can be used:

* a == b - returns *true* if a and b are equal.
* a <> b - returns *true* if a and b are not equal.
* a < b - returns *true* if a is less than b.
* a <= b - returns *true* if a is less than or equal b.
* a > b - returns *true* if a is greater than b.
* a >= b - returns *true* if a is greater than or equal b.
* a and b - returns *true* if a and b are *true*.
* a or b - returns *true* if either a or b or both are *true*.
* not a - returns *true* if a is *false* and *false* otherwise.
* ? a - returns *true* if symbol a exists and *false* otherwise.

The precedence of the operators is as follows:

Highest
* ==, <>, <, <=, >, >=
* not, ?
* and
* or

Lowest

Sub expressions can be enclosed in () to overwrite the operator precedence.

Example:

```
[CPU == "x86-64"]
arch: "64 bit"
[CPU == "x86"]
arch: "32 bit"

tasks:
- [OS == "Windows"]
  build: "cl main.c"
  [OS <> "Windows"]
  build: "gcc main.c"

- [OS == "Windows"]
  clean: "DEL main.exe"
  [OS <> "Windows"]
  clean: "rm ./a.out"
```

Hint: Expressions use the short-circuit evaluation.
