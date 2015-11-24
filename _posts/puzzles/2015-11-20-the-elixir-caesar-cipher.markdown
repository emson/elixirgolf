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

## Solutions

### Winner
`--- your name here! ---`

### Honourable mentions

---

## Resources

* Caesar Cipher on Wikipedia: <https://en.wikipedia.org/wiki/Caesar_cipher>


