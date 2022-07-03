---
layout: post
title: "Compiler Series Part 3: rustc"
tags: compsci compiler
---

This post will be another fly by look at a production compiler, this time for the Rust language.  
*Disclaimer!* I wrote this a while ago and have not checked how much has changed!  The compiler and language are still maturing, so changes are frequent.

The [Rust](www.rust-lang.org) project is a compiler for a new systems language.  The compiler and standard library is written in Rust itself, and the project is sponsored by Mozilla.  It merits itself on being "blazingly fast", preventing segfaults and guaranteeing thread safety.  
Rust is based on multiple paradigms.  It takes its variable mutability/borrowing rules from functional programming, ensuring thread safety and preventing data races.  For example, after passing a variable as an argument into a function, the variable cannot be used again as it is dropped from the scope.  These ownership rules increase the safety of the language, as most potential problems are picked up by the compiler.  If the program compiles, you can be fairly confident that there won’t be many unexpected runtime errors.

The compilation process is different to that of gcc, although the underlying idea is the same.

## Parsing[^1]
Although parsing can be done by writing a finite state machine, the rust parser is hand written.  First, a lexer takes the input source as UTF-8 text and generates tokens from it.  These tokens are then placed into a token tree[^2].  This tree is an intermediate stage between the input source, and the AST.  This stage isn't always necessary, depending on the complexity of the language.  Recursive descent is then used to generate the real AST.  The AST does not include unnecessary bits of the input like parentheses.  They can be implied at this point. 

## Expansion
At this point, the AST is passed over.  At the appropriate locations, external code such as the standard library and other specified modules are injected.  In addition to this, macros are expanded.  This is in contrast to the GCC method, where macros are dealt with by manipulating the source code directly.  Tests (which are also defined with # syntax) are built into a harness at this stage and injected into the AST.  The AST is given node IDs for later use, and optionally can be output at this point.

## Analysis
Semantic analysis happens at this stage.  This involves traversing through the AST and performing different transformations.  At this stage, the AST becomes HIR, _High-Level Intermediate Representation_.  This is basically the same as the AST, so we don’t need to worry about the detail too much.  Many tasks are done here, including but not limited to:
 - Name resolution - checking whether identifiers are variables, functions or modules.
 - Finding the `main` method as this is the entry point for the program
 - Type checking - determining the resultant type of expressions
 - Checking the rules associated with static, constant and private types are obeyed
 - Match checking - Rust’s pattern matching rules are enforced here
 - Dead code checking - emits warnings if unreachable code is detected

These steps are not optimisations as such, more implementations of language features.  

Afterwards, the HIR is translated to MIR, _Mid-Level Intermediate Representation_[^3].  This breaks up the large step between a fairly high level, abstract representation and the final low level representation.  For example, all control flow, loops and matches are represented using `goto` like statements.  Borrow checking is done at this stage, making sure the rules are adhered to, and some optimisations are made before translating MIR into LLVM IR.

## LLVM
LLVM is a set of tools used to help write compilers.  For now we just need to know that it takes a low level intermediate representation of the source code as an input and generates bytecode for the specified platform.  
The first job is to translate Rust’s crates to LLVM’s modules.  "Crates" and "Modules" are similar concepts, it’s just the technicalities of representation that need to be translated.
After this step, LLVM runs its own optimisation passes.  These ensure that the output program is as efficient as possible.  Rust has many implementations of its own optimisations that LLVM could run, but are known to be slow.  For all its efficiency, compile times with LLVM are frequently quite slow.
Once the IR has been optimized, code generation happens.  LLVM either writes object files or assembly in the specified output format to disk.

## Linking
If object files were emitted (eg. if we are building a native library or our project contains multiple files) the final step is to link these files.  This is done by calling the platform’s c compiler (eg. cc) and uses it to link the object files into an executable.

I would like to use LLVM in my compiler, as it a well used system that will make code generation much more straightforward.


[^1]: ["A More Detailed Tour of the Rust Compiler" - Tom Lee](https://tomlee.co/2014/04/a-more-detailed-tour-of-the-rust-compiler/) 
[^2]: [rust/ast.rs at master · rust-lang/rust · GitHub](https://github.com/rust-lang/rust/blob/master/src/libsyntax/ast.rs#L545-L580)
[^3]: ["Introducing MIR" - The Rust Programming Language Blog](https://blog.rust-lang.org/2016/04/19/MIR.html)
