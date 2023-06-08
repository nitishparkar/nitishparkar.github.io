---
title: "The Essential Resources for Mastering Ruby"
---

I was first introduced to Ruby ten years ago when I was hunting for my first job. The take-home project involved learning Ruby and building a simple Rails application. I did get the job, and that set me on a path that involved using Ruby/Rails for the most part for the next ten years. Ruby seems to have fallen out of favor these days, so I don’t often get asked by junior devs about how to improve their Ruby chops. But I am still putting this post out in case there are devs who want to. This post is a collection of resources that helped me improve my understanding of the language and made me a better dev.

## The first step

If you wanted to learn Ruby in 2012, you had two choices: [Humble Little Ruby Book](https://www.infoq.com/minibooks/ruby/) and [Why's (poignant) Guide to Ruby](https://poignant.guide/book/chapter-1.html). The latter's humor and narrative style didn't suit me, so I went ahead with HLRB. It was very concise and helped me get familiar with the language. 

There are several interactive sites for learning Ruby these days, such as [Codecademy](https://www.codecademy.com/learn/learn-ruby), [Learn Ruby Online](https://www.learnrubyonline.org/), and [Ruby Monk](https://rubymonk.com/). Even the [Official Ruby site](https://try.ruby-lang.org/) also has a 30min interactive tutorial that covers the fundamentals.

## Going deeper

Ruby is deceptively powerful. It looks simple, and you can go for a long time without having to use metaprogramming. But the sooner you learn about it, the better your Ruby code will become. And what better resource to do so than [Metaprogramming Ruby](https://www.goodreads.com/book/show/21824181-metaprogramming-ruby-2)?! It’ll introduce you to Ruby’s advanced features. You’ll learn how some of the stuff that made Rails so powerful works underneath.

You've probably heard that in Ruby, everything is an object. There is an entire chapter in the Metaprogramming Ruby book that covers Ruby's object model. I recently discovered this video that brilliantly explains the topic - [A Deep Dive into the Ruby Object Model](https://www.youtube.com/watch?v=by5fFOBhtPQ). Worth watching even if you have read Metaprogramming Ruby. 


## Writing better code

[Avdi Grimm](https://avdi.codes/) is a well-known figure in the Ruby community, known for his expertise in Ruby programming and software craftsmanship. I wish I had read his Ruby books sooner. They are full of practical advice and will help you write better code.

[Confident Ruby](https://pragprog.com/titles/agcr/confident-ruby/)
This book is full of little practical nuggets that you can start applying immediately. If you've been using Ruby for a while, you've probably thought of several of these on your own. This isn't one of those high-level design pattern books; rather, it's something you can apply at the method level. Overall, this book will improve your Ruby skils and make you a more confident Ruby developer.
 
[Exceptional Ruby](https://avdi.gumroad.com/l/NWtnk)
This book is a deep dive into Ruby exceptions. It starts with explanation of Ruby exceptions and exception-handling mechanisms and then suggests some techniques for dealing with them effectively. There is little overlap between this and Confident Ruby. Even though it only covers a small part of the language, it is full of practical insights. I would recommend this if you write Ruby code for a living.

Another book I would like to mention is [Practical Object Oriented Design in Ruby](https://www.poodr.com/). Unlike Avdi's books, this is a bit high-level. Think of it as a book on Ruby design patterns. I didn't enjoy it as much as the other books, but there were some interesting ideas to consider.


## Testing

Every developer who has worked on a codebase with a test suite knows the importance of good tests. Yet very few work on improving how they write tests. RSpec is the most popular testing framework in the Ruby world. Its DSL takes some getting used to, but it is extremely powerful. [Effective Testing with RSpec 3](https://pragprog.com/titles/rspec3/effective-testing-with-rspec-3/) will teach you to use it effectively and to make full use of its capabilities. After going through this book, my tests became much more concise, and needed fewer changes as the code changed.


Ruby and Rails have played an impactful role in web development in the last decade. I have yet to come across a more capable web application framework than Rails. While Ruby and Rails have fallen out of favour in recent years, I remain optimistic that [Rails 7](https://rubyonrails.org/2021/12/15/Rails-7-fulfilling-a-vision)'s return to its roots as "The One Person Framework" will usher in a new era of glory.