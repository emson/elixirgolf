---
layout: post
title: Elixir FizzBuzz Golf
modified:
categories: articles
tags: []
excerpt: Elixir FizzBuzz from 1 to 100
image:
  feature:
share: true
date: 2015-11-17
---

## Instructions

Write an elixir program that will print the numbers from `1` to `100`,
replacing numbers divisible by `3` with `Fizz`,
numbers divisible by `5` with `Buzz` and numbers divisible by both `3` and `5` with `FizzBuzz`.
All other numbers should be returned as normal.

For example:

{% highlight elixir %}
iex> --enter your code here--
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
11
Fizz
13
14
FizzBuzz
..
..
97
98
Fizz
Buzz
{% endhighlight %}

## Solutions

### Winner

The winning solution was submitted by [@andrei_here](https://twitter.com/andrei_here):

`char count: 89`

{% highlight elixir %}
iex(1)> for x<-1..?d,z=(rem(x,3)<1&&"Fizz"||"")<>(rem(x,5)<1&&"Buzz"||""),do: IO.puts z==""&&x||z
1
2
..
..
97
98
Fizz
Buzz
{% endhighlight %}

This solution uses a comprehension `for x<-1..` to iterate over the range of numbers.
The `?d` is a clever trick that saves one character, this is because the character `'d'`
is `100` in ASCII and by using the `?` we get the integer value of the character.

Comprehensions can consist of between 2 and 3 components: a generator, a filter (optional) and a body.
In this solution Andrei abuses the filter, where he builds up the string
`"FizzBuzz"` and binds it to the variable `z`.
The `"Fizz"` string is determined when the range passes a value `x` into the
`rem` function. If `x / 3` then the remainder is `0`. Andrei manages to save one
character by using `<1` instead of `==0`. If `x` is divisible by `3` then the
`&&` returns a `"Fizz"` string otherwise the `||` returns an empty string.

The `<>` concatenates the `"Fizz"` string with a similar `"Buzz"` string, at
which point the body of the comprehension is called. All this does is check to
see whether the `z` is an empty string if so it returns the value of the
variable `x` otherwise it returns the appropriate `"Fizz", "Buzz", "FizzBuzz"`.

This all weighs in at 89 characters! Well done.

To see some of Andrei's other attempts checkout his Gist:
<https://gist.github.com/zyro/e60e75ef3a3c3f08e5ef>


### Honerable mentions

Submitted by [@henrik](https://twitter.com/henrik):

`char count: 99`

{% highlight elixir %}
for n<-1..?d,r=&(rem(n,&1)==0&&IO.write&2),do: (f=r.(3,"Fizz");b=r.(5,"Buzz");f||b||r.(1,n);:io.nl)
{% endhighlight %}

{% highlight elixir %}
for n<-1..?d,r=&(rem(n,&1)==0&&IO.puts&2),do: r.(15,"FizzBuzz")||r.(3,"Fizz")||r.(5,"Buzz")||r.(1,n)
{% endhighlight %}

{% highlight elixir %}
for n<-1..?d,do: Enum.find [{15,"FizzBuzz"},{3,"Fizz"},{5,"Buzz"},{1,n}],fn({d,t})->rem(n,d)==0&&IO.puts(t)end
{% endhighlight %}

Checkout Henrik's Gist and see further explainations:
<https://gist.github.com/henrik/6ca434c0c5cb66ed29d7>

---

Submitted by [@emson](https://twitter.com/emson):

`char count: 129`

{% highlight elixir %}
s=&(&1==1&& &2||String.slice("FizzBuzz",&1==10&&4||0,&1==6&&4||8));Enum.each 1..?d,&IO.puts s.(rem(trunc(:math.pow &1,4),-15),&1)
{% endhighlight %}


This attempt is influenced by the HackerRank article
<https://blog.hackerrank.com/fizzbuzz/>.
Here we create an anonymous function that slices the `"FizzBuzz"` string
in different ways according to the numbers passed to it. The numbers are
generated using [Fermat's little
theorem](https://en.wikipedia.org/wiki/Fermat's_little_theorem), which proved to
be an interesting way of calculating whether the numbers cleanly divide the
values from 1 to 100.

---

## Resources

* Elixir docs `Enum.at`:
  <http://elixir-lang.org/docs/stable/elixir/Enum.html#at/3>
* Comprehension docs `for`:
  <http://elixir-lang.org/getting-started/comprehensions.html>
* More comprehension docs:
  <http://elixir-lang.org/docs/stable/elixir/Kernel.SpecialForms.html#for/1>
* HackerRank FizzBuzz: <https://blog.hackerrank.com/fizzbuzz/>
* Fermat's little theorem:
  <https://en.wikipedia.org/wiki/Fermat's_little_theorem>



