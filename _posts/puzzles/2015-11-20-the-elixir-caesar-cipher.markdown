---
layout: post
title: The Elixir Caesar Cipher
modified:
categories: articles
tags: []
excerpt: Create a Caesar Cipher algorithm and encrypt a message.
image:
  feature:
share: true
date: 2015-11-20
---

## Instructions

Write an elixir [Caesar Cipher](https://en.wikipedia.org/wiki/Caesar_cipher)
program that will encrypt a message.  Your program should prompt the user for
input (`IO.gets`), and they will enter the following:

    number,and a message

* `number` - This is the number of characters to shift the Caesar Cipher e.g.
  `3` will shift the alphabet to `xyzabcdefghijklmnopqrstuvw`
* `message` - This is alphabetic, lowercase and whitespace is permitted (whitespace will
simply map to whitespace of the encoded text).

The output will be lowercase with whitespace.

For example:

{% highlight elixir %}
iex> --enter your code here--
3,abc defg hijklmnopqrstuvwxyz
xyz abcd efghijklmnopqrstuvw
{% endhighlight %}

The solution that completes this puzzle with the shortest number of characters
wins.

### Update

So this weeks elixirgolf probably needed a better definition as there
were some queries around cycling of the letters, and the bounds of the numbers
passed in etc. However it did lead to some very creative answers and as our goal
is to use these puzzles as a learning tool, we are going to be lenient.

## Solutions

### Winner
There are two winners this week, as the rules were unclear on the input bounds.
The first winner was the solution that solved the above example with the
shortest number of characters, regardless of whether the "shift" number should
cycle or not.
The second winner was the shortest "most complete" solution.


### Overall Shortest Winner

Winner: [@gregvaughn](https://twitter.com/gregvaughn)

`char count: 114`

{% highlight elixir %}
# for an offset range of -26 to + 26
[n,p]=IO.gets("")|>String.split",";IO.puts for c<-to_char_list(p),do: c<97&&c||97+rem c-71-String.to_integer(n),26
{% endhighlight %}

First this solution gets the user's input `IO.gets("")|>String.split","`, which
will be the String `3,abc def` and uses the `|>` operator to pass this String
value to the `String.split/2` function. Once split around the `,` the number is
bound to the `n` and the message is bound to the `p` value (`[n,p]`).

The algorithm then takes the message `p` and converts it into a character list,
which is just a list of ASCII integers `to_char_list(p)`. So now @gregvaughn
uses comprehension `for c<-to_char_list(p),do:...` to iterate over each of the
integer values binding them to the variable `c`.

The real work of the algorithm takes place in the body of the comprehension,
where @gregvaughn calculates the new value of the ASCII integer based on the
"shift" number `...do: c<97&&c||97+rem c-71-String.to_integer(n),26`. First the
algorithm checks to see if the ASCII value is less than `97` (the ASCII number for
the letter `a`) if it is then it must be a space or other none alphabetic
character, if it is then that character is returned `c<97&&c`. The logic
reads: if `c` is less than `97` then return `c`.

If `c` is great than `97` then the algorithm must remap the ASCII character value
based on the "shift" number `n`, this is triggered from the `c<97&&c||` (or) operator.
To remap the original letter to the new letter we need to increment the
ASCII value by our value of `n`, but we need to handle values great than 26
(the number of letters in the Alphabet).

First we take the base value of `a` which is `97` and add our "shift" value by
converting our ASCII letter value to a value in the range of `26` using `c-71`,
we then subtract our "shift" number from this value while converting it to an
Integer `String.to_integer(n)`. Finally we find the remainder of `26` using
`rem c-71-String.to_integer(n),26`, and add it to `97`. At last we have the
mapped value of `c` and we convert this character list to a String using
`IO.puts`. If the value of `n` is out of the range `-26..+26` then we get other
printable characters e.g. when `n=30` output is ```]^_ `ab```

A great solution thanks.

### Most Complete Shortest Winner

Winner 2: [@henrik](https://twitter.com/henrik) and [@gregvaughn](https://twitter.com/gregvaughn)  
As @gregvaughn influnenced @henrik's solution he is also named as a winner.

`char count: 121`

{% highlight elixir %}
[n,s]=String.split IO.gets(""),",";IO.write Regex.replace~r/\S/,s,fn<<c>>->?a+rem c-71-rem(String.to_integer(n),26),26end
{% endhighlight %}

### Honourable mentions

Submitted by [@andrei_here](https://twitter.com/andrei_here):

`char count: 88`

{% highlight elixir %}
{_,[n,c]}=:io.fread"",'~d,~s';:io.format"~s",[(for x<-c,do: x>96&&rem(x-97+n,26)+97||x)]
{% endhighlight %}

This solution isn't a winner but is definately in the spirit of elixirgolf,
which is to use puzzles for learning the elixir language.

Unfortunately this solution doesn't cycle the correct way i.e. `abc` should map to `xyz`, when
the shift number is `3`. Also the solution can only handle single words i.e. no
space separation. However it does have some very interesting concepts:

`{_,[n,c]}=:io.fread"",'~d,~s';` Uses the Erlang [io.fread](http://www.erlang.org/doc/man/io.html#fread-2)
function and reads characters from the standard input. The format of the input
is defined as `'~d,~s'` which defines digits, a comma and a single string.
The result of this input is bound to the List variables `n` (for digits) and `c` (for the message) the `:ok`
atom which `io.fread` returns is ignored with the wildcard `_`. Note that the
value of `c` is a character list e.g. `'abc'`.

@andrei_here then uses the character list to change the mapping of the
characters. As a character list is just a list of integers he is able to use a
comprehension to get each value and "shift" it by 26 characters `(for x<-c,do:
x>96&&rem(x-97+n,26)+97||x)`. As we still havd our data as a character list
@andrei_here uses the Erlang function `:io.format` to format the character list
into a string, note that it is wrapped in square brackets.

---

Submitted by [@gregvaughn](https://twitter.com/gregvaughn)

`char count: 114`

{% highlight elixir %}
# for an offset range of -26 to + 26
# 114 characters
[n,p]=IO.gets("")|>String.split",";IO.puts for c<-to_char_list(p),do: c<97&&c||97+rem c-71-String.to_integer(n),26

# for any offset range
# 122 characters
[n,p]=IO.gets("")|>String.split",";IO.puts for c<-to_char_list(p),do: c<97&&c||97+rem c-71-rem(String.to_integer(n),26),26
{% endhighlight %}

<https://gist.github.com/gvaughn/b295e69b4eb302a4fab5>

---

Submitted by [@henrik](https://twitter.com/henrik)

`char count: 124`

{% highlight elixir %}
[n,s]=String.split IO.gets(""),",";IO.write Regex.replace~r/\S/,s,fn<<c>>->q=c-rem String.to_integer(n),26;q<?a&&26+q||q;end
{% endhighlight %}

{% highlight elixir %}
# If we combine "my" Regex.replace with Greg Vaughn's smarter maths, we get to
# 121 together:
[n,s]=String.split IO.gets(""),",";IO.write Regex.replace~r/\S/,s,fn<<c>>->?a+rem c-71-rem(String.to_integer(n),26),26end

# Just for fun: Using Stream.cycle as suggested by Martin Svalin:
[n,s]=String.split IO.gets(""),",";IO.write Regex.replace~r/\S/,s,fn<<c>>->Enum.at Stream.cycle(?z..?a),?z-c+String.to_integer(n)end

# Just for fun: Using Code.eval_string to convert to int and char list at the
# same time… (The "do" block contents are borrowed from Greg Vaughn)
{s,n: n}=Code.eval_string String.replace(IO.gets(""),~r/(.+),(.*)\n/,"n=\\1;'\\2'");IO.puts for c<-s,do: c<97&&c||97+rem c-71-rem(n,26),26

# Just for fun: if we limit to single digits we can do:
<<n,?,,s::bits>>=IO.gets"";IO.write Regex.replace~r/\S/,s,fn<<c>>->97+rem c-71-n+?0,26end

# or even
<<n,?,,s::bits>>=IO.gets"";IO.write Regex.replace~r/\S/,s,fn<<c>>->97+rem c-n-23,26end

# If you don't need to "wrap" (so you can do d->a but not a->x).
# `import String; … split … to_integer` is the exact same char count as not
# importing.
import String;[n,s]=split IO.gets(""),",";IO.write Regex.replace~r/\S/,s,fn<<c>>->c-to_integer(n)end
{% endhighlight %}

For more details see @henrik's gist: <https://gist.github.com/henrik/f7a58f2bc02cd4b20099>


---

Submitted by [@MPAhrens](https://twitter.com/MPAhrens)

`char count: 131`

{% highlight elixir %}
fn[n,m]->for<<x<-m>>,into: "",do: <<(if x<?a,do: 32,else: rem(x+String.to_integer(n)-?a,26)+?a)>>end.(String.split IO.gets(""),",")
{% endhighlight %}

<https://gist.github.com/mpahrens/9262c76aa5388834a0d9>

---
## Resources

* Caesar Cipher on Wikipedia: <https://en.wikipedia.org/wiki/Caesar_cipher>


