---
layout: post
title:  "Poly build instructions"
date:   2019-02-15 01:37:50 +0800
categories: jekyll update
---
According to official site's instruction, you have to run
the following instructions:
{% highlight bash %}
make
make compiler
{% endhighlight %}

What it really does, is:
```bash
# build polyimport from C source ...
# to build basis etc
./polyimport polytemp.txt . < ./exportPoly.sml
# to build compiler
./poly --error-exit < mlsource/BuildExport.sml
```
So, you need to follow those `sml` files to see what's going on.
