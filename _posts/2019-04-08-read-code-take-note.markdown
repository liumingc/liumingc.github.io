---
layout: post
title:  "Read code, take note"
date:   2019-04-08 21:39:00 +0800
categories: jekyll update
---

I think i'm interested in human understanding. So i like to read the books
talking about studying method, like "how to read a book" etc. There is some
basic techniques, but it wont help always. Sometimes you just need to work hard,
there is no tricky way.

To take notes, a picture is sometimes good to illustrated the idea, but not
always. You need words to express your thoughts eventually. Dont too trusted any
specific technique, they have their limits, you should known what's good for,
and what's not. Then choose the right tool to solve your problem.

Math (and maybe programming), is sth that is problem driven. You need some
motivation, to keep you move, to keep you focused. If there is no target, you
will easily lost in the wild. For there are just too many knowledge there, you
can't learn them all. And even if you learn most of them, if you lack the
motivation, and lack some imagination, then you can't create sth.

And you need to focus on the specific problem at one time.
For example, when you read the following code,
```sml
local
  fun addToActive(r, l) =
  (
    case Vector.sub(regStates, r) of
      {prop=RegPropStack _, ...} => l
    | _ => if Array.sub(pushArray, r) then l else r :: l
  )
in
  fun nowActive regs = List.foldl addToActive [] regs
end
```
Then i may want to grep the code to search the definition of `RegPropStack`. But
does the definition matters so much that it can distract you to something
else? What benefits do you get after reading the definition? It seems that it's
  not that important. The code should be self-contained at the moment.
You dont need to known the whole type, you just seem this part of type, which is
sufficient for current code. When you need more, you will see them later. So
just dont think that big now.
type annotation `datatype regProperty` doesn't help that much then simply
`{prop=RegPropStack _, ...}`, and that is the idea of information hiding.

And reading books, most of the time, is a more effetive way to learn than
reading code. Because there too many subtle details in code, you are not
interested in them.

If you want to known how does graph coloring can handle the problem of register
allocation, then you should read the essential part, ignore other details.
