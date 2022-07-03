---
layout: post
title: "Compiler Series Part 1: Introduction"
tags: compsci compiler
---
Programming was my first foray into computer science, and recently I wanted to dig into this topic in more detail.  The posts which follow will be a collection of my notes, made while writing my own compiler in C++.

Compilers are an essential part of computing, allowing higher level languages to be translated to machine code that the computer can understand.  This provides an important level of abstraction to the programmer, meaning they don’t have to worry about the complexity and quirks of assembly languages.  
I will break down the compiler into a collection of separate, easier to understand modules.  Compilers lend themselves to this modular approach quite nicely.  From front to back:
1. The lexer - This performs lexical analysis, breaking the input up into tokens
2. The parser - This puts the tokens we get from the lexer into the context of our language via an intermediate representation
3. Transformation - Once we have the intermediate representation, this can be tidied and optimised
4. Code generation - The intermediate representation is used to generate the required assembly for each language construct

In the next section, we'll get a bird's eye view of [gcc](https://gcc.gnu.org/) the GNU Compiler Collection. 
