{% include mathjax %}

# Y combinator, or the fix point function

Y combinator is used for recursive function definition.
It cannot be expressed in a typed language like ML, it has to be a built in primitive in ML.

But in untyped language, or a more powerful typed language, maybe you can expressed it as a normal function.

## Intuition

```
facterial = fn n =>
	if n = 0
	then 1
	else n * facterial(n-1)
```

But if you can't refer facterial in the body, then how to write a similar function?

You can pass a recur fact' function as an argument:

```
fact = fn f => fn n =>
		if n = 0
		then 1
		else n * f (n-1)
```

Then what is f?

`f` should contain `fact`.

You need a function, which takes it's self as an argument.

So, there should be a form `M M n`
where

```
M = fn M => fn n => Z
```

Here, z should be

```
fact X n
```

What is `X` ?

`X` is just

```
X = fn n =>
	M M n
```

So, put the whole things together, we get the following body:

```
(fn M => fn n => fact X n)
```

By \\( \eta \\) reduction, it can be written as

```
(fn M => fact X) (fn M => fact X)
```

replace X,

```
(fn M => fact (fn n => M M n)) (fn M => fact (fn n => M M n))
```

generalize it, or to abstract it, 

```
Y = fn fact =>
	(fn M => fact (fn n => M M n))
	(fn M => fact (fn n => M M n))
```

or

\\[
Y = \lambda f . \
		(\lambda M . f (\lambda n . M M n))
		(\lambda M . f (\lambda n . M M n))
\\]