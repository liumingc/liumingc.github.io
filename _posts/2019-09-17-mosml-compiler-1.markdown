I want to add some debug flag to the mosml compiler, so i can see the intermediate output.
How does mosml exports val/fun/modules?

Emmm, i don't know. But then i realise that i dont need to export those things,
i just need to add a flag to to compiler.

So here is what i changed,

`launch/mosmlc.tpl`:

```diff
while : ; do
	case $1 in
	# ...
+	-pr)
+		compopt="$compopt $1";;
	esac
	shift
done
```

Then in `compiler/Mainc.sml`:

```diff
fun enable_pr () =
	Compiler.pr_flag := true

fun main() =
(
	Arg.parse [
+		("-pr", Arg.Unit enable_pr)
		]
)
```

and `compiler/Compiler.sml` and `compiler/Compiler.sig`(the latter one is trivial so omit here),
```diff
+ val pr_flag = ref false

fun compLamPhrase os state (RE, lams) =
(
	app
	(fn (is_pure, lam) => 
		(
+		if !pr_flag then ( msgIBlock 0; Pr_lam.printLam lam; ...) else ();
		))
	lams
);
```

That's it. Rebuild mosml, and try it out:

```sml
(* fib.sml *)
fun fib 0 = 1
  | fib 1 = 1
  | fib n = fib(n-1) + fib(n-2);
```
The output:
```sh
mosmlc -c -pr camlCase.sml                                                      (base) 
let  in unspec end
***kph_is_pure*** = true;
***kph_inits*** = 
***kph_funcs*** = 
(prim (set_global camlCase.camlCase/1) (fn let (prim (makeblock 250:1) (BLOCK 1:2 )) in letrec (fn let (app (prim (get_global TextIO.input1/0) ) (prim (get_global TextIO.stdIn/0) )) in ((switch:2 var:0 of 0:2 : (BLOCK 0:1 )) statichandle ((case (prim (field 0) var:0) of #"\n" : (BLOCK 0:1 )) statichandle (if(app (prim (get_global Char.isAlpha/0) ) (prim (field 0) var:0)) then (if(prim (field 0) var:3) then (((app (prim (get_global TextIO.print/0) ) (app (prim (get_global Char.toString/0) ) (app (prim (get_global Char.toUpper/0) ) (prim (field 0) var:0)))); (prim (setfield 0) var:3 (BLOCK 0:2 )))) else (app (prim (get_global TextIO.print/0) ) (app (prim (get_global Char.toString/0) ) (app (prim (get_global Char.toLower/0) ) (prim (field 0) var:0))))) else if(prim (test:eq) (prim (field 0) var:0) #"_") then ((prim (setfield 0) var:3 (BLOCK 1:2 ))) else (app (prim (get_global TextIO.print/0) ) (app (prim (get_global Char.toString/0) ) (prim (field 0) var:0))); (app var:2 (BLOCK 0:1 ))))) end) in (app var:0 (BLOCK 0:1 )) end end))
***kph_is_pure*** = true;
***kph_inits*** = closure 2 0; set_global camlCase.camlCase/1
***kph_funcs*** = label 1; get_global TextIO.stdIn/0; push; get_global TextIO.input1/0; apply 1; push; access 0; branchif 8; (BLOCK 0:1 ); return 2; label 8; access 0; getfield 0; branchinterval 10 10 7 7; (BLOCK 0:1 ); return 2; label 7; access 0; getfield 0; push; get_global Char.isAlpha/0; apply 1; branchifnot 5; envacc 1; getfield 0; branchifnot 6; access 0; getfield 0; push; get_global Char.toUpper/0; apply 1; push; get_global Char.toString/0; apply 1; push; get_global TextIO.print/0; apply 1; envacc 1; push; (BLOCK 0:2 ); setfield 0; branch 3; label 6; access 0; getfield 0; push; get_global Char.toLower/0; apply 1; push; get_global Char.toString/0; apply 1; push; get_global TextIO.print/0; apply 1; branch 3; label 5; access 0; getfield 0; push; #"_"; test:noteq 4; envacc 1; push; (BLOCK 1:2 ); setfield 0; branch 3; label 4; access 0; getfield 0; push; get_global Char.toString/0; apply 1; push; get_global TextIO.print/0; apply 1; label 3; (BLOCK 0:1 ); push; envacc 0; appterm 1 3; label 2; (BLOCK 1:2 ); makeblock 250:1 1; push; access 0; closurerec 1 2; push; (BLOCK 0:1 ); push; access 1; appterm 1 4
```
That's nasty, can do better. But it's enough for today.