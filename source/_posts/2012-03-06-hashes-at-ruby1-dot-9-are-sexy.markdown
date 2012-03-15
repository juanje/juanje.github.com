---
layout: post
title: "hashes at Ruby1.9 are sexy"
date: 2012-03-06 02:36
comments: true
categories:
- ruby
- Ruby 1.9
---

Probably everybody knew that, but I didn't when I readed at the [Programming Ruby](http://pragprog.com/book/ruby3/programming-ruby-1-9) book.

I was use to the no so sexy syntax for the hashes in Ruby:

```ruby
numbers = { "one" => "uno", "two" => "dos", "three" => "tres" }
```

Although is very common to see them at code using symbols as keys and split the elements into lines:

```ruby
numbers = {
  :one   => "uno",
  :two   => "dos",
  :three => "tres"
}
```

which, actually, looks nicer... (at least to me)

But in Ruby 1.9 they decide that if this was so common, maybe it could me a good idea simplify it. So now we can do the same by doing:

```ruby
numbers = {
  one:   "uno",
  two:   "dos",
  three: "tres"
}
```

It is less verbose and, I think, more natural.
