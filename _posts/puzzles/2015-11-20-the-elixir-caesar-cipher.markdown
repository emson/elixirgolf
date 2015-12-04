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

So this weeks **elixirgolf** probably needed a better definition as there
were some queries around cycling of the letters, and the bounds of the numbers
passed in etc. However it did lead to some very creative answers and as our goal
is to use these puzzles as a learning tool, we are going to be lenient.

## Solutions

There was a lot of buzz around the Caesar cipher puzzle. Many of the
solutions were very creative and influenced one another. As a result the winning
solution was an evolution of ideas. Therefore we will give credit to all those who
contributed.


### Winner

All though not initially the winner @henrik persevered and managed to produce a
working Caesar cipher in a staggering 103 characters! His solution
was built from his own concepts and inspiration from both @gregvaughn and @MPAhrens.

Winner: [@henrik](https://twitter.com/henrik)

`char count: 103`

{% highlight elixir %}
[n,p]=IO.gets("")|>String.split",";IO.puts for<<c<-p>>,do: c<97&&c||97+rem c-71-String.to_integer(n),26
{% endhighlight %}

[@henrik](https://twitter.com/henrik) uses the same approach as
[@gregvaughn](https://twitter.com/gregvaughn) to extract the shift number and
message, as well as calculating the mapping cycle (see Greg's solution for a detailed description).

`...for<<c<-p>>,do:...` Henrik uses the trick [@MPAhrens](https://twitter.com/MPAhrens) developed
to convert the message `p`, into a character list. He does this by wrapping the entire
comprehension body in a binaray e.g. `<< "a" >>`. Notice how the comprehension
body `c <- p` is wrapped in the binary, this saves 12 characters.

Well done and also a big well done to both @gregvaughn and @MPAhrens

### Second Place

[@gregvaughn](https://twitter.com/gregvaughn) was in the pole position with his solution.
Although the rules were a bit fuzzy around the bounds of the "shift" number, his solution
met the basic criteria. It also influenced the winning solution.

Second: [@gregvaughn](https://twitter.com/gregvaughn)

`char count: 114`

{% highlight elixir %}
# for an offset range of -26 to + 26
[n,p]=IO.gets("")|>String.split",";IO.puts for c<-to_char_list(p),do: c<97&&c||97+rem c-71-String.to_integer(n),26
{% endhighlight %}

First this solution gets the user's input `IO.gets("")|>String.split","`, which
will be the String `3,abc def` and uses the `|>` operator to pass this String
value to the `String.split/2` function. Once split around the `,` the number is
bound to `n` and the message is bound to the `p` variable (`[n,p]`).

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
based on the "shift" number `n`, this is triggered from the `||` (or) operator.
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

### Honourable mentions

Submitted by [@andrei_here](https://twitter.com/andrei_here):

`char count: 88`

{% highlight elixir %}
{_,[n,c]}=:io.fread"",'~d,~s';:io.format"~s",[(for x<-c,do: x>96&&rem(x-97+n,26)+97||x)]
{% endhighlight %}

This solution isn't a winner but is definately in the spirit of **elixirgolf**,
which is to use puzzles for learning the [elixir
language](http://elixir-lang.org).

Unfortunately this solution doesn't cycle the correct way i.e. `abc` should map to `xyz`, when
the shift number is `3`. Also the solution can only handle single words i.e. no
space separation. However it does have some very interesting concepts:

`{_,[n,c]}=:io.fread"",'~d,~s';` Uses the Erlang [io.fread](http://www.erlang.org/doc/man/io.html#fread-2)
function that reads characters from the standard input. The format of the input
is defined as `'~d,~s'` which defines digits, a comma and a single string.
The result of this input is bound to the List variable `n` (for digits) and `c` (for the message) the `:ok`
atom which `io.fread` returns is ignored with the wildcard `_`. Note that the
value of `c` is a character list e.g. `'abc'`.

@andrei_here then uses the character list to change the mapping of the
characters. As a character list is just a list of integers he is able to use a
comprehension to get each value and "shift" it by 26 characters `(for x<-c,do:
x>96&&rem(x-97+n,26)+97||x)`. As we still have our data as a character list
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

The solution provided by @MPAhrens works as follows. He creates a giant
anonymous function that he calls, inline, with an `IO.gets ""` call which he
splits. The function broken down looks like this:

    fn [n,m] -> [n,m] end.(String.split IO.gets(""),"," )

He then uses a comprehension that uses the very interesting technique of
wrapping the entire body in a binary `<<x<-m>>` this has the benfit of
converting the message string `m` to a character list and binding each integer
to `x`. His comprehension also uses the `into: ""` function, which returns a
function that collects values alongside the initial accumulation value.
Basically for each iteration of the comprehension the character value is
appended to the string `""`.

The comprehension body then does the mapping calculation:

{% highlight elixir %}
<<(if x<?a,do: 32,else: rem(x+String.to_integer(n)-?a,26)+?a)>>
{% endhighlight %}

Again he wraps the entire body in a binary `<<>>`, then he uses an `if/else`
function to determine if the message character value is below the letter
`a` (ASCII `97`) if it is, it assumes that it is a space character and the ASCII value
`32` is returned. Otherwise @MPAhrens calculates the new character mapping.

The calculation finds the remainder `rem(X_VALUE,  26)` of the current character ASCII value
`x` and adds the "shift" number (while converting it to an integer) less the
ASCII value of `?a`. The result is then added to the ASCII value of `?a`.

Great solution and we really liked the interesting use of binaries.

<https://gist.github.com/mpahrens/9262c76aa5388834a0d9>

---

## Resources

* Caesar Cipher on Wikipedia: <https://en.wikipedia.org/wiki/Caesar_cipher>
* Erlang `:io.fread`: <http://www.erlang.org/doc/man/io.html#fread-2>
* Comprehension docs `for`:
  <http://elixir-lang.org/getting-started/comprehensions.html>
* More comprehension docs:
  <http://elixir-lang.org/docs/stable/elixir/Kernel.SpecialForms.html#for/1>
* Elixir `into` collectable function:
  <http://elixir-lang.org/docs/stable/elixir/Collectable.html#into/1>
* Elixir `<<>>/1` bitstrings:
  <http://elixir-lang.org/docs/stable/elixir/Kernel.SpecialForms.html#%3C%3C%3E%3E/1>


