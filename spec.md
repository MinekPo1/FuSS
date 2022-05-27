# Specification

Commands in FoSS are split with newlines.

## Types, literals and expressions

There are three types in FoSS:

- `void`
- `num`
- `str`

`void` literals can take the form of `null` or `NULL`

`num` literals can take the form of

- `true` or `false` (equal to `1` and `~1`)
- an integer (`123` for example)
- a decimal (`123.456` for example)

Simple `str` literals can be made by encapsulating some characters with quote marks (`"abc"` for example)

Multi line `str` literals begin with `*"`, each line must begin with a quote and ends with `"*`

For example

```FuSS
*"This is a multi line string literal
 "Yay
 "Isn't this great?
 "*
```

Since FuSS has no comments, string literals are used as comments.

In expressions, these literals, along with functions and macros, can be combined with operators:

| operator | rtype | ltype | note |
| :------- | :---: | :---: | :--- |
| `+`      | `num` | `num` | adds the two numbers
| `-`      | `num` | `num` | subtracts the left number from the right one
| `*`      | `num` | `num` | multiplies the two numbers
| `:`      | `num` | `num` | divides the two numbers
| `;`      | `num` | `num` | gets the reminder of the division of the two numbers
| `&`      | `num` | `num` | preforms the bitwise and on the two numbers
| `|`      | `num` | `num` | preforms the bitwise or on the two numbers
| `^`      | `num` | `num` | preforms the bitwise xor on the two numbers
| `~`      |   -   | `num` | negates the left number
| `<`      | `num` | `num` | if the left number is grater than the right the expression can be simplified to 1, else it can be simplified to -1
| `>`      | `num` | `num` | if the right number is grater than the left the expression can be simplified to 1, else it can be simplified to -1
| `=`      | `num` | `num` | if the right number is equal than the left the expression can be simplified to 1, else it can be simplified to -1
| `%`      | `str` | `str` | joins the two strings
| `#`      | `str` | `str` | if the string are equal simplify to 1 else to -1
| `{`      | `str` | `num` | simplify to only the character with the index defined by the number from the string and every character after that
| `}`      | `str` | `num` | simplify to only the character with the index defined by the number from the string and every character before that
| `@`      | `str` |   -   | simplify to the length of the string as a number
| `?`      | `num` | `str` | if the number is positive, simplify to the string. Else simplify to a empty string.
| `\`      | `num` | `num` | if the right number is positive, simplify to the left number. Else simplify to 0.

Operators are preformed left to right.

You can also use brackets to group expressions.

## Calling functions

The only way to store data in FoSS is via functions.

A function is called as you would expect: by following the function name with brackets.

```FuSS
f()
```

Inside of the brackets arguments may be passed, split by colons.

```FuSS
f(1,2,3)
```

To access the service value of a function, the function name is used.
Note that the service value of a function can be gathered directly after calling it.

## Defining functions

A function definition starts with the `static func` keyword. After that, the function service type and name is specified, with the type being first, followed by brackets, in which arguments may be specified, with the argument types and names in them, separated by commas. Argument types precede their names and are separated by one or more spaces.
If a argument has a default value, after its name, the value is placed, prefixed by one or more space.

After the brackets the body is placed, surrounded by square brackets.

To serve a value, the `serve` keyword is used.

```FuSS
static func num f(num a 1, num b 2, num c 3)[
        serve a + b + c
]
```

Functions may have multiple definitions which differ by the argument count and type.

Functions can also serve members. To do so a member must first be defined. This is done by adding an arrow `->` after the brackets, but before the body, followed by a member definition.
A members definition is first the type, followed by the member name, and the optional default value. Each member definition must be on its own line.
All members with out default values must be served before the function serves.
If a function serves the type void, the function serves if all members have been served.
To serve a member, the `to` keyword follows the serve statement, itself being followed by the member name.

```FuSS
static func num f2(num a 1, num b 2, num c 3)
        -> num a
        -> num b
        -> num c
[
        serve a to a
        serve b to b
        serve c to c
        serve a + b + c
]
```

To get the value of a member, the function name is followed by the member name split with a dot.

```FuSS
f2(2,3,4)

f2
"9"

f2.a
"2"
```

Note that in this case the function returns a structure. See [Structures](#structures) for more information.

## Defining macros

If you with to specify a value with out defining a function, a macro may be used.
A macro is created by first placing the `static macro`, followed by the macros type and the name of the macro.

```FuSS
static macro num a 10
```

A macro can hold multiple values, if it has a `multi` keyword before the `macro` keyword.
To add new values, repeat the macro definition.

```FuSS
static multi macro num b 1
static multi macro num b 2
static multi macro num b 3
static multi macro num b 4
```

Every line with a multi macro will be duplicated, for each value to be used.

```FuSS
f(b)
"f will be called with the argument 1, then 2, 3 and 4"
```

NOTE: Macros are not constant variables, their value is calculated every time they are used.

```FuSS
static macro num c f()
c + c
"f is called twice"
```

## `until` loop

```FuSS
until c [
        "code"
]

```

The `until` loop will execute the code in the brackets until the condition is:

- a non empty string
- a positive number

## Structures

To define a structure first the `static structure` keywords are used followed by the main structure type and name.
Then members are defined, alike function members.

```FuSS
static structure num Struct
        -> num a
        -> num b
        -> num c
```

To define a macro of type or function serving a structure, place `structure <name>` instead of the type.

```FuSS
static macro structure Struct d
```

Structures which have the same member name and types are assumed to be equal.

If a function is marked as serving a structure it has to serve all members of the structure.
If the function defines its own members, the returned structure will be different from the one marked.

```FuSS
static func structure Struct f_s()
        -> num d
[
        serve 1 to a
        serve 2 to b
        serve 3 to c
        serve 4 to d
        serve 5
]

static macro structure Struct d f_s
"^ type error"
```

## Importing and namespaces

To import a library place the library name in between `<` and `>`.

The library serves as a namespace, hence the macros and functions defined within can be accessed with a dot.

```FuSS
<sysio>

stdio.print("hello world!")
```

If you wish to merge a imported, or created namespace, the `merge` keyword is used.
After the keyword, the namespace you wish to merge is placed.
Optionally this may be followed by the `to` keyword and another namespace to combine those two namespaces. Otherwise the namespace is merged with the default namespace.

```FuSS
<sysio>
merge sysio

print("hello again!")
```

To create a namespace, use the `static namespace` keyword, followed by the namespace name, after which the namespace body in square brackets should be placed.

```FuSS
static namespace Namespace [
        static macro int a 40
]
```

## Naming

Function, macro, namespace and structure names can contain:

- lower and uppercase letters
- numbers, aside from the first character
- underscores

Names of functions and macros **MAY NOT** overlap.
