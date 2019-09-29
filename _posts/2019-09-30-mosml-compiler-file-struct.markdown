Consider we are under `src/compiler` this directory.

Mixture contains some environment related functions.

I'll try to show the basic flow of the byte code compiler:

```
 source code
     |
     |
Parser:
  Parser
     | Asynt (Abstract SYNCTax)
     v
Elab: 
   Elab

   Resolve
     | Asynt
     v
Front:
   translateTopLevelDec
     | Lam
     v
Back:  
  compileLamPhrase
     | Zam
     v
Emit: 
  emitPhrase
     | Byte code instruction
     v
```

I annotated the intermediate result(Asynt/Lam/Zam) aside the flow arrows.

Elab is trying to build the `ModEnv/FunEnv/SigEnv/VarEnv/TyEnv` etc.

And maybe it will try to annotate the expression with types(not check yet).
