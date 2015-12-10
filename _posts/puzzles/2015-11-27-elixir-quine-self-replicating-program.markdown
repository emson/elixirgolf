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

### Winner

Winner: [@rubysolo](https://twitter.com/rubysolo)

`char count: 46`

{% highlight elixir %}
q=<<"q=~p;:io.format q,[q]">>;:io.format q,[q]
{% endhighlight %}

#### Understanding
The key to this Quine is the Erlang function `:io.format/2`. This function
takes 2 parameters `Format` and `Data`, which it uses to write the Data to
the standard output device in accordance to the Format provided.

For example, using the Format `"~p"`, it will write the Data using standard syntax
and will output lists of printable characters as strings. Interestingly the Data
will replace the `~p`value, this means you can surround it with other text for example
`"hello ~p world"`.

{% highlight elixir %}
iex> :io.format "hello ~p world", ["my"]
hello <<"my">> world:ok
{% endhighlight %}

NB. the `:ok` atom is just the standard return response so we can ignore it for
our Quine purposes.

Next we build the String to be passed to `:io.format`. This String is
the clever bit, as it will be crafted in such a way that `:io.format` will use
it to substitue the `~p` value with itself. If we substitue the Data parameter
with `"BOOM"` you can see what `:io.format` does. It converts the `["BOOM"]`
list to a binary (String) `<<"BOOM">>`. Notice that the `<<>>` **elixir**
bitstrings are returned.

{% highlight elixir %}
iex> :io.format "q=~p;:io.format q,[q]", ["BOOM"]
q=<<"BOOM">>;:io.format q,[q]:ok
{% endhighlight %}

As `<<>>` bitstrings are returned we need to surround our Format String with
them. Finally we bind the whole value to the variable `q`, and remember to
separate the 2 conditions with `;`.

A great solution, which demonstrates the symbiotic relationship between Erlang and
**elixir**.



### Honourable mentions

Honourable mention solutions are ordered by influence rather than on char count.
This is because often a particular solution will influence others.

Submitted by [@Phani_Mahesh](https://twitter.com/phani_mahesh):

`char count: 90`

{% highlight elixir %}
"fn x->IO.puts~s(\#{inspect x}|>\#{x}) end.()"|>fn x->IO.puts~s(#{inspect x}|>#{x}) end.()
{% endhighlight %}

#### Understanding

Typically a Quine takes a String of code and rebuilds it with a "builder"
function. This "builder" function reassembles the code when it is executed.
In the solution @Phani_Mahesh has provided, he uses the following function,
`fn x -> IO.puts ~s(#{inspect x}|>#{x}) end.()`. If we tidy it up and pass in a
String value we can see what it does:

{% highlight elixir %}
iex> f = fn x -> IO.puts ~s(#{inspect x}|>#{x}) end
#Function<6.54118792/1 in :erl_eval.expr/5>
iex> f.("BOOM")
"BOOM"|>BOOM
:ok
{% endhighlight %}

As we can see it uses the `|>` pipe operator to pipe a string into a non-String
version of itself. This is interesting as @Phani_Mahesh's function 
`fn x -> IO.puts ~s(#{inspect x}|>#{x}) end.()` has an empty anonymous function
invocation applied to the end `end.()` which takes no parameters. Actually it
does take parameters as the `|>` is passing the String value into the first
arity position, therefore there is no need to declare it.

The function also uses the `~s` sigil to return a String representation of
whatever is within it's brackets `~s(#{inspect x}|>#{x})`. Note that the `#{}`
interpolation will be applied, and that we can use `inspect` without prefixing
the module e.g. `IO.inspect`. So the `inspect` will return the String surrounded
by `""` quotes.

Finally the String used to pass to the function is almost an exact
representation of the function, apart from the escape characters `\`. These will
prevent the String from interpolating the value between the `#{}` characters.

{% highlight elixir %}
iex> "fn x->IO.puts~s(\#{inspect x}|>\#{x}) end.()"
"fn x->IO.puts~s(\#{inspect x}|>\#{x}) end.()"
{% endhighlight %}

Excellent, well done.

---

Submitted by [@CoderDennis](https://twitter.com/coderdennis):

`char count: 82`

{% highlight elixir %}
"(&(IO.puts~s(\#{inspect&1}|>\#{&1}))).()"|>(&(IO.puts~s(#{inspect&1}|>#{&1}))).()
{% endhighlight %}

#### Understanding

This solution is based on @Phani_Mahesh's solution, however @CoderDennis has
taken it further and realised that he can use the anonymous function capture
syntax.

Thus the function:

{% highlight elixir %}
"fn x -> IO.puts ~s(#{inspect x} |> #{x}) end.()"
{% endhighlight %}

Becomes:

{% highlight elixir %}
(&(IO.puts ~s(#{inspect&1} |> #{&1}))).()
{% endhighlight %}

Here `x` becomes the capture parameter `&1`, and the function syntax `fn x -> x
end` becomes `&(&1)`. Finally the whole anonymous function is invoked with
`.()`, for example, `(&(&1)).("BOOM")`.

Good thinking, well done.

---



## Resources

* Wikipedia Quine
  <https://en.wikipedia.org/wiki/Quine_%28computing%29>
* Erlang `io.format/2`: <http://www.erlang.org/doc/man/io.html#format-2>
* Elixir docs `<<>>/1` bitstrings:
  <http://elixir-lang.org/docs/stable/elixir/Kernel.SpecialForms.html#%3C%3C%3E%3E/1>
* Elixir docs interpolation & anonymous functions:
  <http://elixir-lang.org/getting-started/basic-types.html>
* Elixir docs sigils: <http://elixir-lang.org/getting-started/sigils.html>



