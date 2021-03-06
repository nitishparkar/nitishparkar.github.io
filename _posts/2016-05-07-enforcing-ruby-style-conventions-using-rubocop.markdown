---
title: "Enforcing Ruby style conventions using RuboCop"
---

[RuboCop](https://github.com/bbatsov/rubocop) is a Ruby static code analyzer. It can be used to enforce guidelines outlined in [Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide). It's particularly useful when you are working in a team and everyone is using their favourite editor. RuboCop README is very detailed. It may seem a bit daunting at first, so I will explain basic setup and a few commands to help you get started.

RuboCop installation is pretty straightforward. Add it to the Gemfile and run `bundle install`

{% highlight ruby %}
group :development, :test do
  gem 'rubocop', require: false
end
{% endhighlight %}

There are two command flags that I use the most.

{% highlight bash %}
# Displays failing cop names in offense messages.
# Useful in understanding which cops are failing.
rubocop -D

# Auto-correct certain offenses. Particularly useful for auto-aligning code.
# This is experimental, so always verify the changes it makes.
rubocop -a
{% endhighlight %}

If you run rubocop command in an existing project for the first time, you are likely to be greeted with a large number of offences. It can be overwhelming to correct all those offences at once. Fortunately, there is a way by which you can make these corrections step-by-step.

{% highlight bash %}
# Run this in your project's root directory
rubocop --auto-gen-config
{% endhighlight %}

This command generates `.rubocop_todo.yml`. Comment inside the file perfectly describes its purpose:

> This configuration was generated by
> `rubocop --auto-gen-config`
> on 2016-04-27 23:32:28 +0530 using RuboCop version 0.39.0.
> The point is for the user to remove these configuration records
> one by one as the offenses are removed from the code base.
> Note that changes in the inspected code, or installation of new
> versions of RuboCop, may require this file to be generated again.

Then create `.rubocop.yml` configuration file which inherits from .rubocop_todo.yml. Here is a sample configuration file:

{% highlight yaml %}
inherit_from: .rubocop_todo.yml
AllCops:
  Include:
    - Rakefile
    - config.ru
  Exclude:
    - db/schema.rb
    - vendor/**/*
    - bin/**/*
{% endhighlight %}

And a snippet from RuboCop README which explains it's options:

> RuboCop checks all files found by a recursive search starting from the directory it is run in, or directories given as command line arguments. However, it only recognizes files ending with .rb or extensionless files with a #!.*ruby declaration as Ruby files. Hidden directories (i.e., directories whose names start with a dot) are not searched by default. If you'd like it to check files that are not included by default, you'll need to pass them in on the command line, or to add entries for them under AllCops/Include. Files and directories can also be ignored through AllCops/Exclude.

RuboCop has a [sample config](https://github.com/bbatsov/rubocop/blob/master/config/default.yml) file which explains all the configuration options in detail.


If you run RuboCop at this point, it should print 'no offenses detected'. If it detects any offenses, manually add them to `.rubocop_todo.yml`


Now you are all set to start fixing offences. Make sure you commit everything and all the tests are passing at this point. Go to `.rubocop_todo.yml`. Pick one offence you want to deal with and remove it from the file. Try auto-fixing it with `rubocop -a`. If it doesn't get fixed, fix it manually. If you are unsure about how to fix it, run `rubocop -D`. It will tell you the name of the failing cop. Google it and you will find details on how to fix it. If you don't approve of a cop, you can disable it from the config file(`.rubocop.yml`). For example:

{% highlight yaml %}
Metrics/ClassLength:
  Enabled: false
{% endhighlight %}

Keep doing this till you have cleared `.rubocop_todo.yml`

You can go one step ahead and make RuboCop part of your build flow. For example, if you are using [CircleCI](https://circleci.com/), you can run RuboCop before your tests

{% highlight yaml %}
# circle.yml
test:
  pre:
    - rubocop -D
{% endhighlight %}

This will fail build if RuboCop detects any offenses. Might annoy your teammates a bit, but will be worth it.

That's it. I have covered only the most basic configuration. Once you are comfortable with RuboCop, delve deeper. It has excellent README.


*Thanks to [Mohit Thatte](https://twitter.com/mohitthatte) for introducing RuboCop at [Simpl](https://getsimpl.com).*
