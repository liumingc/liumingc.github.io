---
layout: post
title:  "latex note"
date:   2020-02-10 10:58:00 +0800
categories: jekyll update
---

## skeleton

```latex
% !TEX program = xelatex
\documentclass{article}
\begin{document}
\end{document}
```

Document class can be: article, paper etc.

## To support Chinese

```latex
usepackage{ctex}
% \begin{document}
```

## TOC

```latex
\usepackage{hyperref}
% \begin{document}
\tableofcontents
\chapter{Chap 01}
\section{First}
\subsection{Question rising}
```

## To use code blocks

```latex
\usepackage{listings}
\lstset{
	basicstyle=\small\ttfamily,
	frame=shadowbox
	%frame=single
}
% \begin{document}

\begin{lstlisting}[language=C]
int foo;
\end{lstlisting}
```

## Figures, pictures

### metapost

`mpost <filename>.mp` to generate the eps(by default) file, then,

```latex
\usepackage{graphics}
% \begin{document}
\includegraphics{foo.1}
```
to include the figures.
And you can use `\begin{figure} ... \end{figure}` to surround the figure/graph.

### tikz

## Cross refs

```latex
\footnote{This is a footnote.}


\label{item:one}
\ref{item:one}
```

## Handling packages

Use `tlmgr` to find out information about package.

```bash
dnf search texnames # find nothing
tlmgr info texnames
#  Packages containing files matching `texnames':
#  beebe:
#        texmf-dist/tex/generic/beebe/texnames.sty
sudo dnf install beebe # tlmgr recommend using dnf to install the package
```

## How to convert tex into pdf(The XXX Book)?

Relace
```tex
\loop\iftrue
  \errmessage{...}
\pausing1 \input manmac
```
with
```tex
\loop\iffalse
  \errmessage{...}
\input manmac
\newif\ifproofmode
\proofmodefalse
```

## Some useful commands

To show some special words,
`\verb|your text|` or `\begin{verbatim} ... \end{verbatim}` to surround the text.

## TODO

Equations, table.

## Quetions

- How to find doc for a command?
