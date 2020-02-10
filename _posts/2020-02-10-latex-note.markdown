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

### tikz

## Cross refs

```latex
\footnote{This is a footnote.}


\label{item:one}
\ref{item:one}
```

## TODO

Equations, table.

## Quetions

- How to find doc for a command?
