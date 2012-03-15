---
layout: post
title: "Some Ruby regex tips"
date: 2012-03-15 20:39
comments: true
categories: 
- ruby
- regex
---
I haven't been fan to the regular expresion myself, but they are very useful and I try to learn each day to use their potencial.

I going to write down here some tips that I have found useful and I like to remember. I hope this helps anyone else.

First it's important to remember that the `Strings` are very powerfull _per se_ and sometimes there is no need for a regular expresions. For example:

```ruby
1.9.3-p0 :000 > 'Here is some ramdom text'.include? 'ramdom'
 => true 
```

There are a lot of interesting methods for `Strings` and I love them, but let's see some cases for regex:

The tipical base case is the previous one, check if there is some text or pattern inside one string. This can be done by doing:

```ruby
/ramdom/ =~ 'Here is some ramdom text'
```

This will return the position in the string where the pattern start and `nil` if the pattern doesn't match. You can use it in a `if` stament to see if this match. The positive value that indicate the position will count as `true` for the `if`, so we can follow with our workflow.

The string that match with the pattern will be stored in a special var. From `$1` to `$9` are reserved for this purpose. But, nine vars? Yes, because we can search for more than one pattern and store their values in different variables. Those patterns are called _groups_ and are delimited by parentesis. Lets see it:

```ruby
/.*(first).*(second)/ =~ 'Something first and second'
1.9.3-p0 :001 > /.*(first).*(second)/ =~ 'Something first and second'
=> 0 
1.9.3-p0 :002 > $1
=> "first"
1.9.3-p0 :003 > $2
=> "second" 
```

Oviously is is very simple and silly example. Well this is very basic, but what I wanted to talk about is how to work better with those groups and special variables.
Groups are used for capturing text, but also to define alternatives in a patterns. For example:

```ruby
files = %w( home.png party.jpg party_music.mp3 )
#  => ["home.png", "party.jpg", "party_music.mp3"] 
photos = files.select { |f| /\w+\.(png|jpg)/ =~ f }
#  => ["home.png", "party.jpg"]
```

So the pattern will match eigther with `/\w+\.png/` or `/\w+\.jpg/`. This is very useful and is tipical from any language or tool that support regex, but is good to know.
But one interesting thing that I have learnt recently is how to avoid to feed the special variables with the matches' results.

```ruby
# We have those sentences:
1.9.3-p0 :001 > senteces = [
1.9.3-p0 :002 >   'John is crazy',
1.9.3-p0 :003 >   'Anne is crazy',
1.9.3-p0 :004 >   'Peter is crazy',
1.9.3-p0 :005 >   'John is kidding'
]
#  => ["John is crazy", "Anne is crazy", "Peter is crazy", "John is kidding"]

# We want to check what is says about 'John' or 'Anne'
1.9.3-p0 :006 > senteces.each do |sentence|
1.9.3-p0 :007 >   if /(John|Anne) is (\w+)$/ =~ sentence
1.9.3-p0 :008 >     puts "#{$1} -> #{$2}"
1.9.3-p0 :009 >   end
1.9.3-p0 :010 > end
John -> crazy
Anne -> crazy
John -> kidding
```

But there is a nicer way to save the matches, we can store them in our own variables:

```ruby
1.9.3-p0 :011 > senteces.each do |sentence|
1.9.3-p0 :012 >   if /(?<name>John|Anne) is (?<wat>\w+)$/ =~ sentence
1.9.3-p0 :013 >     puts "#{name} -> #{wat}"
1.9.3-p0 :014 >   end
1.9.3-p0 :015 > end
John -> crazy
Anne -> crazy
John -> kidding
```

And you also can ignore any group. I found this useful at the `step_definitions` for _Cucumber_. Here is one example I did time ago:

```ruby
When /^the calculator (divide|substract) the (\d+) (?:by|to) the (\d+)$/ do |command, arg1, arg2|
  @output = `ruby bin/rcalc #{command} #{@first} #{@second}`.chomp
  $?.success?.should be_true
end
```

Cucumber checks for this regex againts the _features_ and pass the groups founded to the block (as `command`, `arg1` and `arg2`). In this perticular case I want a expresion that matched with that kind of sentence but I didn't want to store `by` or `to`. Mostly beacuse Cucumber highlight the groups matched at the output and I didn't want for those words. I just wanted reuse the regex to both cases.
Anyways, I found `(?:regex)` interesting and useful.

Well, I don't know if someone find this useful, but at least I got this written down as alternative to my bad memory :-P

Oh, btw, If you are going to use regular expresions in Ruby I higly recomended you [Rubular](http://rubular.com/). Which is an awesome online Ruby regular expresion editor. Has some nice quick reference and check on-the-fly all your expresions and cases. Really cool :-)

