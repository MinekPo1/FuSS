# Spefification

Commands in FoSS are split with newlines.

## Types, literals and expressions

There are three types in FoSS:

- `void`
- `num`
- `str`

Here are all the ways to create a literals:

| type |    pattern    | example |
|:-----|:-------------:|--------:|
|`void`| `(NULL|null)` |   `NULL`|
|`num` |     `\d+`     |  `12345`|
|`num` |   `\d+.\d+`   |  `12.34`|
|`num` |`(true|false)` |   `true`|
|`str` |`"([^"]|\\")*"`| `"abcd"`|

In expressions, these literals, along with functions and macros, can be commbined with operators:

| operator | rtype | ltype | note |
| :------- | :---: | :---: | :--- |
| `+`      | `num` | `num` | adds the two numbers
| `-`      | `num` | `num` | subtracts the left number from the right one
| `*`      | `num` | `num` | multiplies the two numbers
| `:`      | `num` | `num` | devides the two numbers
| `;`      | `num` | `num` | gets the reminder of the devision of the two numbers
| `&`      | `num` | `num` | preforms the bitwise and on the two numbers
| `|`      | `num` | `num` | preforms the bitwise or on the two numbers
| `^`      | `num` | `num` | preforms the bitwise xor on the two numbers
| `~`      |   -   | `num` | negates the left number
| `<`      | `num` | `num` | if the left number is grater than the right the expression can be simpilfied to 1, else it can be simplifed to -1
| `>`      | `num` | `num` | if the right number is grater than the left the expression can be simpilfied to 1, else it can be simplifed to -1
| `=`      | `num` | `num` | if the right number is equal than the left the expression can be simpilfied to 1, else it can be simplifed to -1
| `.`      | `str` | `str` | joins the two strings
| `#`      | `str` | `str` | if the string are equal simplify to 1 else to -1
| `{`      | `str` | `num` | simplyfy to only the character with the index defined by the number from the string and every character after that
| `}`      | `str` | `num` | simplyfy to only the character with the index defined by the number from the string and every character before that
| `@`      | `str` |   -   | simplyfy to the lenght of the string as a number
| `?`      | `num` | `str` | if the number is positive, simplyfy to the string. Else simplify to a empty string.
| `\`      | `num` | `num` | if the right number is positive, simplyfy to the left number. Else simplify to 0.

Operators are preformed left to right.

## Calling functions

The only way to store data in FoSS is via functions.

A function is called as you would expect: by following the function name with brackets.

```FuSS
f()
```

Inside of the brackets arguments may be passed, split by collons.

```FuSS
f(1,2,3)
```

To access the service value of a function, the function name is used.

## Defining functions

A function definition starts with the `static func` keyword. After that, the function service type and name is specified, with the type being first, followed by brackets, in which arguments may be specified, with the argument types and names in them, seperated by commas. Argument types preceed their names and are seperated by one or more spaces.
If a argument has a defult value, after its name, the value is placed, prefixed by one or more space.

After the brackets the body is placed, surrounded by square brackets.

To serve a value, the `serve` keyword is used.

```FuSS
static func num f(num a 1, num b 2, num c 3)[
	serve a + b + c
]
```

Functions may have multible definitions which differ by the argument count and type.


## Defining macros

If you with to spefify a value with out defining a function, a macro may be used.
A macro is created by first placing the `static macro`, followed by the macros type and the name of the macro.

```FuSS
static macro num a 10
```

## Importing and namespaces

To import a library place the library name in between `<` and `>`.

The library serves as a namespace, hence the macros and functions defined within can be accessed with a dot.

```FuSS
<stdio>

stdio.print("hello world!")
```

If you wish to merge a imported, or created namespace, the `merge` keyword is used.
After the keyword, the namepace you wish to merge is placed.
Optionaly this may be followed by the `to` keyword and another namespace to combine those two namespaces. Otherwise the namespace is merged with the defult namespace.

```FuSS
<stdio>
merge stdio

print("hello again!")
```