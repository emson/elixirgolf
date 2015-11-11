---
layout: post
title: Number to Even/Odd String
modified:
categories: articles
tags: []
excerpt: Use local input to enter an integer and return a string of each digit being even/odd
image:
  feature:
share: true
date: 2015-11-11
---

## Instructions

Use the `IO.gets` function to gather an integer from user input and return
a string where EACH digit is converted to an `"e"` or an `"o"` string character.
This string character is determined by whether the integer digit at that index
is odd or even.

For example:

{% highlight elixir %}
iex> --enter your code here--
13387
"oooeo"
{% endhighlight %}

## Solutions

### Winner

The winning solution was submitted by [@henrik](https://twitter.com/henrik):

{% highlight elixir %}
iex(1)> "#{for x<-(to_char_list String.strip IO.gets''),do: Enum.at'eo',rem(x,2)}"
12231
"oeeoo"
{% endhighlight %}

Starting in the middle it `IO.gets` the user's input, strips off the
new line character `\n` using `String.strip`. It then converts this binary
string into a character list (a list of integer values).

It uses a comprehension (`for x<-...`) to take each integer from our list
and pass it through to the body of our comprehension `do: Enum.at....`

In the body, the integer value bound to `x` is divided by `2` determining whether it is even or
odd. This `rem`ainder value is then used by `Enum.at` to return the
corresponding `"e"` or `"o"` character.

Finally the code is wrapped in an interpolated string `"#{}"` to convert the
character list into a binary string.


### Honerable mentions

Submitted by [@henrik](https://twitter.com/henrik):

{% highlight elixir %}
"#{for x<-'13387',do: Enum.at'eo',rem(x,2)}"
{% endhighlight %}

{% highlight elixir %}
"#{Enum.map'13387',&%{0=>?e,1=>?o}[rem(&1,2)]}"
{% endhighlight %}

{% highlight elixir %}
"#{for x<-'13387',do: Enum.at'eoeoeoeoeo',x-48}"
{% endhighlight %}

---

Submitted by [@meadsteve](https://twitter.com/meadsteve):

{% highlight elixir %}
import String;for s<-:stdio|>IO.gets|>codepoints,s !="\n",x=s|>to_integer|>rem(2),do: Enum.at'eo',x
{% endhighlight %}

---

## Resources

* Elixir docs `Enum.at`:
  <http://elixir-lang.org/docs/stable/elixir/Enum.html#at/3>
* Comprehension docs `for`:
  <http://elixir-lang.org/getting-started/comprehensions.html>
* More comprehension docs:
  <http://elixir-lang.org/docs/stable/elixir/Kernel.SpecialForms.html#for/1>
* String interpolation:
  <http://elixir-lang.org/getting-started/basic-types.html#strings>



