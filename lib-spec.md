# standard library specification for FuSS

## sysio

### void print(str s)

print the text to the console

### void putc(num a)

print the character with the ascii value a to the console

### str input()

take input from the console and serve it

### num getc()

take a single character from the console and serve its ascii value

### num getX()

serve the column of the console cursor

### num getY()

serve the row of the console cursor

### void setXY(num x, num y 0)

set the position of the console cursor
if x and y are not intygers, they will be rounded down

## strmod

### str toStr(num n, num base)

represent the number as a string

### num fromStr(str s, num base)

convert the string to a number

### str replace(str s, str old, str new)

serve a copy of s with every instance of old replaced with new

## sysmath

### num abs(num n)

If n is negative, serve ~n, otherwise serve n

### num sqrt(num n)

Serve the square root of n

### num pow(num n, num p)

Serve n to the power of p
