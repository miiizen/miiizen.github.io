---
layout: post
title: "Compiler Series Part 2: GCC"
tags: compsci compiler
---

[GCC](https://gcc.gnu.org/), originally written for the GNU operating system is a set of front ends, libraries and back ends for a few different programming languages.  This is an old piece of software, but is kept up to date and used regularly by many.  The compiler has front ends for (can compile) C/C++, Objective C, Fortran, Ada and Go.  
The compiler was originally built to avoid paying for a license to use a vendor compiler for the GNU operating system.  Since then, it has has risen past proprietary compilers in both popularity and performance.
The process started when a user runs a command like `gcc myfile.c` at the terminal can be broken down into 4 steps: preprocessing, compilation, assembly and linking.

## The preprocessor[^1]
The preprocessor deals with making modifications to the source code before it is compiled.  We will be looking at the C/C++ preprocessor specifically.  Other preprocessors or frontends are available for different languages as mentioned above.  A certain amount of lexing occurs at this step.  The actions it performs are:

### Line splicing
This tidies the lines of the source up if escaped newlines are used.  Escaped newlines are backslashes used to break up lines.  They must appear between tokens or within comments.
All comments within the code are also replaced with single spaces.

### Tokenisation
Once these transformations are complete, the program is split up into preprocessor tokens.  These tokens are going to be passed to the compiler, where they will be used as compiler tokens.  For now, the preprocessor tokens can be put into 5 categories: identifiers, preprocessing numbers, string literals, punctuators, and other.

 - Identifiers - any sequence of letters, digits or underscores which DOES NOT begin with a number.  Keywords - tokens such as if, else are treated as identifiers.  This means that we can define macros with the same name as reserved keywords in the actual C/C++ language

 - Preprocessing numbers - The definition encompasses a large range of representations of numbers.  In order to support hex and other representations, any combination of letters, numbers, periods and underscores are allowed after a decimal digit.  This allows many invalid forms to slip through this step.  They will be picked up by the compiler however.

 - String literals - This includes string constants, character constants and arguments for the #include statement wrapped in angular brackets.

 - Punctuators - All punctuation in ASCII except from ‘@’, ‘$’, and ‘’’ are considered punctuators. Punctuators have meaning to the compiler, but what that meaning is depends on the context in which they are found.

### Directive and macro handling
This is the step most people associate with the C/C++ preprocessor, as it is seen most often by the user of the compiler.  If the stream of tokens contains nothing in the preprocessing language, it is simply passed to the compiler.  
The following are common feature used in the preprocessing language.

#### Including header files
Header files contain C/C++ declarations and other preprocessor instructions.  Header files can be provided by the OS in order to use the system API, or defined by the user.  This makes it possible to share definitions between files.
If we were to look at the code output by all the intermediate steps, we would see any `#include <xyz.h>` replaced with the contents of `xyz.h`

#### Macro handling
Macros can be object-like or function-like.  Object-like macros look like data objects.  They are most commonly used to give names to symbolic constants eg. `#define MAX_SIZE = 1024`
Where the name `MAX_SIZE` is used in our code, the preprocessor will replace this text with `1024`

Function-like macros look like function calls when used.  When they are “called” the code that appears in the definition is simply copied to where it has been called. Eg. 
`#define SQUARE(x) x*x`
Wherever `SQUARE(X)` appears in code, it is replaced with `x*x`, where x is the value passed into square.
Macros are used to increase performance.  Because the code is just copied by the preprocessor, in the assembly output there is no overhead with function stacks etc.  This paragraph barely scratches the surface of macros and how useful they can be, however, they are not an essential part of a compiler.

#### Conditional compilation
This allows the preprocessor to decide whether to hand the following section of code to the compiler.  There are a few situations in which this is useful, for example include guards.  Include guards prevent the same header file being copied too many times.  This speeds up compilation.  
```c
// An example include guard
#ifndef __MY_HEADER
#define __MY_HEADER
...
#endif
```
## Compilation[^2]
This is the stage which takes our preprocessed code, and generates assembly from it.  The gcc compiler makes multiple passes over representations of the input program, transforming it into different intermediate representations before outputting machine code. 
The figure below shows the steps involved[^3]
![https://upload.wikimedia.org/wikipedia/commons/0/0b/Gcc.JPG](/assets/img/gcc-diagram.jpeg)

### Parsing pass
This is the frontend of the compiler.  The details about the C preprocessor written above apply to this step.  After the preprocessor has done its job, the compiler generates an Abstract Syntax Tree (AST) from the token stream.  For now this can be thought of as a tree data structure that represents the input program.  The AST is very easy to modify and transform, so is useful as an internal representation of the input.  

Each front end works in a different way, but usually ends up outputting a form called Generic (or GIMPLE).  The history and internal workings around this stage seem to be a little complicated and confused, but as long as Generic is output and passed to the compiler we are happy.

### Gimplification pass
This pass involves transforming our Generic representation into a GIMPLE representation.  This is another intermediate representation however, more restrictive than an AST. It is used for optimization passes later on.  At its most basic, GIMPLE is a collection of tuples representing Generic expressions.  The salient points on their structure are as follows:
 - There can be no more than three operands per expression.  If there are more than three, the expression is split into smaller parts.
 - All control flow is made up of conditionals and goto statements.

### Tree SSA
This step involves multiple passes over the internal representation we now have of the code.  SSA stands for _Single Static Assignment_.  The GIMPLE representation is modified so that each variable is only assigned to once.  If a variable needs to be assigned to multiple times, new variables are created.
As can be seen in section 9.4 of the gcc internals manual referenced above, this step involves 47 different optimizations.  These involve optimizing loops, conditionals, removing unreachable code and other complex looking operations. A new GIMPLE representation with changes applied is returned from this step and passed onto the next.

### RTL
RTL stands for _Register Transfer Language_, and is a step closer to our desired machine code output.  The GIMPLE representation is translated to RTL, which assumes an abstract processor with an infinite number of registers.  Many passes are made over this form, optimizing the code even further.  The optimized form is passed to a gcc backend.

## Assembly
Just like frontends, gcc has support for many backends that deal with outputting platform specific machine code.  These include x86, i386 and arm.  When given the optimized RTL, the backend will emit the assembly for the platform specified.  The most commonly used backend is the GNU assembler known as gas or as. This was released in 1986 but is still being used frequently.
The output from assembly is an object file.  This is passed to the linker.

## Linking[^4]
The compilation and assembly process is performed on individual files.  The linker is used to connect all parts of a project together, whether that be header and implementation files or external libraries.  If a function is defined in a different file with extern or similar, it is the linker’s job to check if it has actually been defined.  In a project with thousands of lines of code, recompiling everything for one tiny change in a file would be a big pain.  Instead, the file containing the change is recompiled and the project is relinked.  
Typically on a GNU/Linux system, the program ld is used.  This is the GNU linker.  The gcc program wraps calls to ld to make life easier for the developer, but if necessary this can be done manually.  


This was just a glimpse at the huge, huge GCC project - currently sitting at just under 20 million lines of code. Picking through the technical documentation helped me get an overview of how a compiler should be structured.  After this analysis, I could begin to plan my own compiler. 


[^1]: ["The C Preprocessor"](https://gcc.gnu.org/onlinedocs/cpp.pdf)
[^2]: ["GCC Internals"](https://gcc.gnu.org/onlinedocs/gccint.pdf)
[^3]: ["GCC Internals/GCC Architecture - Wikibooks."](https://en.wikibooks.org/wiki/GNU_C_Compiler_Internals/GNU_C_Compiler_Architecture)
[^4]: ["c++ - How does the compilation/linking process work? - Stack Overflow."](https://stackoverflow.com/questions/6264249/how-does-the-compilation-linking-process-work)
