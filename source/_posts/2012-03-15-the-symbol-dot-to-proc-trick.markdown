---
layout: post
title: "The Symbol.to_proc Trick"
date: 2012-03-15 03:18
comments: true
categories: 
- ruby
- tricks
- Ruby 1.9
---
Sometimes I find myself saying: "must be a better way to do this". And sometimes, there are :-)

This is the case of processing the elements of a list. Let's see it in an example:

I have a object `shop_list` which is a string with a list of element separated by comas
```ruby
1.9.3-p0 :002 > shop_list 
 => "oranges, eggs, oat, water" 
```

when I split them I get:
```ruby
1.9.3-p0 :003 > shop_list.split(',')
 => ["oranges", " eggs", " oat", " water"] 
```

But some of the new elements of the list have a space before them. And I need the list with no spaces to send it to another object, method, app or whatever which doesn't like those extra spaces.
I can remove them like this:

```ruby
1.9.3-p0 :004 > shop_list.split(',').map { |e| e.strip }
 => ["oranges", "eggs", "oat", "water"] 
```

But at Ruby 1.9 we have a simpler way to do the same and is more concise:

```ruby
1.9.3-p0 :005 > shop_list.split(',').map(&:strip)
 => ["oranges", "eggs", "oat", "water"] 
```

So this:
```ruby
# name_args = "oranges, eggs, oat, water"
shop_list = name_args.split(',').map do |item|
  item.strip
end
```

is the same as this:
```ruby
# name_args = "oranges, eggs, oat, water"
shop_list = name_args.split(',').map(&:strip)
```

The reason is because there is a method called `to_proc` that convert an object prefixed with an ampersand (`&`) in a method to call. Ruby will see the symbol (`:strip`) and it will try to convert it into a Proc to call the block with the list from the `map` and invoking the method `strip` at each one of the elements from the list.

Ok, this is not the best explanation ever but I like the new concise form :-)
