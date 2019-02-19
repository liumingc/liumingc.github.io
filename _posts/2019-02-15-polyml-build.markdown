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

# TypeChecking
It seems that typecheck is done when parsing.<br/>
It's hard to lookup the entry point of TYPE_TREE functor/structure, i only found
some makeParseTypeXXX api being called in PARSE_DEC.<br/>

# Code generation
## codetree
In CodeTree/BaseCodeTreeSig.sml,
```sml
datatype codetree =
  Newenv of codeBinding list * codetree
| Constnt of macnineWord * Universal.universal list
| Extract of loadForm
| Indirect of {base: codetree, offset: int, indKind: indKind }
| Eval of (* Evaluate a function with an argument list *)
  {
    function: codetree,
    argList: (codetree * argumentType) list,
    resultType: argumentType
  }
| Unary of {oper: ..., arg1: codetree}
| Binary of {oper: ..., arg1: codetree, arg2: codetree}
| Arbitrary of ...
| Lambda of lambdaForm
| Cond of codetree * codetree * codetree (* If-statement *)
| BeginLoop of ... (* tail-recursive inline function *)
| Loop of (codetree * argumentType) list (* tail-recursive function *)
| Handle of {exp: codetree, handler: codetree, exPacketAddr: int}
| Tuple of {fields: codetree list, isVarient: bool}
| SetContainer of ...
| TagTest of ...
| LoadOperation of ...
| StoreOperation of ...
| BlockOperation of ...
| GetThreadId
| AllocateWordMemory of {numWords: codetree, flags: codetree, initial: codetree}

and loadForm =
  LoadArgument of int
| LoadLocal of int
| LoadClosure of int
| LoadRecursive

and lambdaForm =
{
  body: codetree
  isInline: inlineStatus, (* modified by optimiser *)
  name: string,
  closure: loadForm list,
  argTypes: (argumentType * codeUse list) list,
  resultType: argumentType,
  localCount: int,
  recUse: codeUse list
}
```
From parsetree to codetree, some forms is gone, i.e,<br/>
```
Ident -> ???
structure/signature/functor -> ???
```
Too many duplicate declarations and definitions, it's hard to find definitions.
So sad.

# The big picture

The overview doc says, there are 4 major passes:
- parsing
- type-checking
- code-generation
- optimise & transform to machine code

In STRUCTURES_.ML, you will see there is some comment like `Code-generation
phase`, `Second pass`. So this is the clue to follow and findout TypeChecking.
Or, by reading COMPILER_BODY.ML, the flow is as follow:
```sml
  val parseTree: STRUCTURES.program =
      PARSEDEC.parseDec (...)
  let
      (* pass 2, match ident and declares *)
      val () = STRUCTURES.pass2Structs (parseTree, lex, Env globals)
  in
      let
          (* pass 3, code generation *)
          val (structCode, nLocals) = STRUCTURES.gencodeStructs(parseTree, lex)
      in
          (* pass 4 *)
          let
              val resultCode = CODETREE.genCode(structCode, debugSwitches,
              nLocals)
              fun executeCode() = STRUCTURES.pass4Structs (resultCode (),
              parseTree)
          in
              (SOME(...), SOME executeCode)
          end
      end
  end
```

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
The `rootFunction`, depends on arguments, may call compiler to handle <br/>
`--script <filename>`, or call `shell()` to enter REPL mode.<br/>
And, `PolyML.compiler` has different phases, the first one is in<br/>
`mlsource/MLCompiler/COMPILER_BODY.ML`.<br/>
and the last one, is redefined in `basis/FinalPolyML.sml as follow:
```
fun polyCompiler (getChar: unit -> char option, parameters: compilerParameters list) = ...
...
structure PolyML =
struct
  open PolyML
  val compiler = polyCompiler
  ...
end
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

19/2/2018

The reading progress is slowing down. That's OK, i think. Besides reading
the code, i'm also reading some books(i.e "Engineering a compiler"). And in the
near future, i may write some test code, which will take more time, which will
even slow down more. So take it easy! After all, reading is not everything. Even
if i've done reading all the source code, that won't mean that i'm in a new
  stage. I still have to write many code to learn how to program, how to write a
  compiler.

# Q&A
Q: How to print pass info, how to print parsetree after pass etc?<br/>
A:<br/>
After reading `PolyMLCompiler.html`, and i found there is some flags:<br/>
```sml
val parsetree: bool ref
val codetree: bool ref
val codetreeAfterOpt: bool ref
val assemblyCode: bool ref
val pstackTrace: bool ref
```
So, if i want to check the parsetree, just set it to true:
```sml
PolyML.Compiler.parsetree := true
```
Q: How to do optional arg in SML?<br/>
A:<br/>

Q: There is no Set in polyml, how to do set-related algorithms in sml? And how
to manage libraries in polyml?<br/>
A: There is a IntSet structure in CodeTree/X86Code/IntSet.sml.

Q: When does the pattern-match form get compiled into if-else?<br/>
A:<br/>

Q: How does polyml do pretty-print?<br/>
A:<br/>
