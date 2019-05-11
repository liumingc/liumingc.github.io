---
layout: post
title:  "system reinstall"
date:   2019-05-11 23:13:00 +0800
categories: jekyll update
---

My latop's filesystem is almost out of space now, i'm gonna to reinstall it
sooner or later.
If i reinstall the system, i should do the following things.

## Clean up the file hierarchy

Suppose we are in $HOME now:

```
Documents/
	compiler
	maths
		gtm
	lang
		sml
		ocaml
		haskell
		go
		js/ts
		julia
		go
opt
	polyml
		bin
		lib
	scheme
		bin
		lib
```

## Src

```
redis
nginx
linux
polyml/sml/hamlet/mlton
julia
python
lua
bucklescript
ChezScheme
csl
haskell-impl/ghc
plan9
postgresql
sbcl
SDL
```

Maybe i list too many, i can't read them all.

## Upgrade system

```sh
sudo dnf upgrade --refresh
sudo dnf install dnf-plugin-system-upgrade
sudo dnf system-upgrade download --refresh --releasever=30 # --allowerasing
sudo dnf sytem-upgrade reboot
```
