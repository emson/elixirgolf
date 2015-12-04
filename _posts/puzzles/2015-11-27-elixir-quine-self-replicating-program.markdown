---
layout: post
title: Elixir Quine Self-Replicating Program
modified:
categories: articles
excerpt: Create an Elixir Quine, Self-Replicating Program
tags: []
image:
  feature:
date: 2015-11-27
---

## Instructions

Write an elixir quine. A [Quine](https://en.wikipedia.org/wiki/Quine_%28computing%29)
is a computer program which takes no input and produces a copy of its own source code as its
only output. They are "self-replicating" or "self-reproducing" programs, and
your task is to either create the **shortest** or the most **creative** elixir quine.

There will be two winners, one for the shortest and the other for the most
creative. Creative is open to interpretation, however these are the sort of things we
consider creative:

* best use of language (elixir or erlang), maybe use processes
* interpretation of a quine, e.g. prints the output in reverse
* maybe switches between languages elixir/erlang

The rule is that you enter your code into `iex` and press return, the output
will be your code. This can then be copied and pasted back into `iex` and will
produce the same output. It maybe that your code requires pasting back into
`iex` multiple times, but at some "reasonable" count it should reproduce the
original code.

A Ruby example might be:

{% highlight ruby %}
irb> lambda{|x| puts x + x.inspect}.call"lambda{|x| puts x + x.inspect}.call"
lambda{|x| puts x + x.inspect}.call"lambda{|x| puts x + x.inspect}.call"
=> nil
{% endhighlight %}

#### No cheating quines

Quines, per definition, cannot receive any form of input, including reading a
file, which means a quine is considered to be "cheating" if it looks at its own
source code.

This JavaScript example cheats by calling itself with the `a()` call.
Essentially in this case the `function` code is a file.

{% highlight ruby %}
function a() {
    document.write(a, "a()");
}
a()
{% endhighlight %}

**Please submit your solutions as comments to the Github issue:**  
**<https://github.com/emson/elixirgolf/issues/1>**

## Solutions
[n,p]=IO.gets("")|>String.split",";IO.puts for<<c<-p>>,do: c<97&&c||97+rem c-71-String.to_integer(n),26
### Winner

`--- your name here ---`

### Honourable mentions

---

## Resources

* Wikipedia Quine
  <https://en.wikipedia.org/wiki/Quine_%28computing%29>



