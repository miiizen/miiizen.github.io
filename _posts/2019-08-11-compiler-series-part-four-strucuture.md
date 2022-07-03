---
layout: post
title: "Compiler Series Part 4: Designing the SIMPLE language and compiler"
tags: compsci compiler
---

I've chosen to call the language I've designed for the compiler SIMPLE for fairly self evident reasons.  This language is Turing complete as it features conditional statements and variables.
As this compiler is an MVP, there will only be one data type: doubles. 

## Program structure
The program should be enclosed in `BEGIN..END`. The top level should contain function definitions with a main function as an entry point.

```
BEGIN
   DEFINE average(x, y)
       (x + y) * 0.5
   ENDDEF

   DEFINE main()
       average(5, 8)
   ENDDEF
END
```

## Functions
Functions enclose reusable blocks of code. They can take arguments (must be doubles) and return a value (also a double)

```
DEFINE average(x, y)
       (x + y) * 0.5
ENDDEF
```

There is no ‘return’ keyword, the evaluation of the last expression is the return value of the function.

Functions can be defined as external. This means that a definition exists elsewhere and will be linked later.
```
DEFINE EXT printd(x)
DEFINE EXT sin(x)
```
This allows the language to use functions from the c standard library and any user defined functions which may also be linked.

## If/else statements
If statements decide which block of code to execute based on the result of a conditional.
```
IF 5 < 6 THEN
   ...
ELSE
   ...
ENDIF

IF 4==2 THEN
   ...
ENDIF
```

## For loops
For loops take the form of an identifier (loop variable), end condition and an optional step value.
```
FOR i = 1, i < n, 1 IN
   ...
ENDFOR
```

## Putting it all together
Once finished, the compiler will be capable of emitting an executable for this program.
```
BEGIN
 DEFINE EXT sin(x)
 DEFINE EXT putchard(x)

 DEFINE main()
   num = 7
   FOR y = 1, y >= -1, -0.2 IN
     FOR x = 0, x <= num, 0.2 IN
       s = sin(x)
       IF (0.1+y) >= s THEN
         IF (y-0.1) <= s THEN
           putchard(42) # “*“
         ELSE
           putchard(32) # “ ”
         ENDIF
       ELSE
         putchard(32) # “ “
       ENDIF
     ENDFOR
     putchard(10) # “\n”
   ENDFOR
 ENDDEF
END
```
The program's output is an approximate sketch of a sin wave in the terminal!
![a very rough sin wave!](/assets/img/sin.png)

## Compiler process
A compiler lends itself to a modular approach.  One can take this to the extreme, for example see the paper on a 'nanopass' approach to compiler design[^1].

A level 1 DFD outlining the processes happening within my compiler is included below.  It is a linear process,
![level 1 DFD](/assets/img/dfd.png)
When a language feature is added to the compiler it's quite straightforward to extend each section to support it.

In the next section, we'll get into writing the lexer.


[^1]: [A Nanopass Framework for Compiler Education](https://www.cs.indiana.edu/~dyb/pubs/nano-jfp.pdf)
