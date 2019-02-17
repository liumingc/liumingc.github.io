---
layout: post
title:  "Poly build instructions"
date:   2019-02-15 01:37:50 +0800
categories: jekyll update
---
According to official site's instruction, you have to run<br/>
the following instructions:
```bash
make
make compiler
```

What it really does, is:
```bash
# build polyimport from C source ...
# to build basis etc
./polyimport polytemp.txt . < ./exportPoly.sml
# to build compiler
./poly --error-exit < mlsource/BuildExport.sml
```
So, you need to follow those `sml` files to see what's going on.

And I'm reading about polyml's code.<br/>
The code is quite compact and readable.<br/>
# Lexing
I should read `LEX_.ML`. The most important type is:
```sml
type lexan =
  {
    stream: uint -> char option,  (* get char *)
    ch: char ref,
    sy: sys ref,  (* AndSy, DatatypeSy, etc ... *)
    id: string ref,
    pushedSym: sys ref, (* to push back lookahead sy ... *)
    (* ... more fields omitted *)
  }
```
For example, `insymbol` will eat the next input symbol
```sml
fun insymbol (state as {sy, pushedSym, ...}:lexan) =
  if ! pushedSym <> OtherSy
  then
    pushedSym := OtherSy
  else
  (
    sy := OtherSy
    parseToken state
  )
```
In the firs case, `pushedSym` is not empty, which means that<br/>
there is a push back before. So we reset it, so we eat the symbol.<br/>
Otherwise, we call `parseToken` to get a new sym from the token stream.

# Parsing

# Startup flow
I get the information by greping. That's not convinietn, but i dont<br/>
have a better parsing tools.
```
main - libpolymain/polystub.c
  polymain  - libpolyml/mpoly.cpp
    InitModules
    CreateHeap()
    rootFunction = InitHeaderFromExport(exports)
    StartModules
    processes->BeginRootThread(rootFunction)
    finish(0)
```
where
```
// _exportDescription* -> PolyObject
InitHeaderFromExport - libpolyml/savestate.cpp
```
Actually, rootFunction is in `basis/TopLevelPolyML.sml`.
```
$ grep -r rootFunction *
basis/build.sml:val () = Bootstrap.use "basis/TopLevelPolyML.sml" (* Add
rootFunction to Poly/ML *)
basis/TopLevelPolyML.sml: fun rootFunction () : unit = ...
```


# Reading progress
17/2/2018
I have read `PARSE_DEC.ML`, mark it. I will come back to figure out some<br/>
details. The structure: <br/>
```
ml_bind
  Boot
    Address
    HashTable
    ...
  MLCompiler
    Make
      Lex
      CompilerBody
        Lex
        SymSet
        ParseDec
      Debug
      Pretty
      UniversalTable
      StructVals
```

Now, what if i want to print verbose pass info? i.e, if i want to print<br/>
the parsetree after pass, how can i do that? I need to follow the calling<br/>
flow.
