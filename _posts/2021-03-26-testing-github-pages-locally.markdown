---
layout: post
title: "Testing github pages locally"
date: 2021-03-26 13:32:20 +0300
description: Testing github pages locally
img:  # Add image post (optional)
---
[Install Ruby](https://jekyllrb.com/docs/installation/macos/)

Here are steps I've used on Mac M1 to install rbenv , initiliase it & install Ruby version 2.7.1
{% highlight ruby %}
brew install rbenv
rbenv init
eval "$(rbenv init -)"
RUBY_CFLAGS="-w" rbenv install 2.7.1
rbenv global 2.7.1
ruby -v
{% endhighlight %}

Verify Ruby environment is ok via "rbenv-doctor" : 
{% highlight ruby %}
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-doctor | bash
{% endhighlight %}

You would see something like :
{% highlight ruby %}
Checking `rbenv install' support: /opt/homebrew/bin/rbenv-install (ruby-build 20220910.1)
Counting installed Ruby versions: 1 versions
Checking RubyGems settings: OK
Auditing installed plugins: OK
{% endhighlight %}

For ruby 3, you might see "webrick" error. You can apply folloring [workaround](https://github.com/jekyll/jekyll/issues/8523)

{% highlight ruby %}
gem "webrick"
{% endhighlight %}

[Install Jekyll & Bundler](https://jekyllrb.com/docs/installation/)
{% highlight ruby %}
sudo gem install --user-install bundler jekyll
{% endhighlight %}

Install packages : 

{% highlight ruby %}
bundle install
{% endhighlight %}

Serve content locally : 

{% highlight ruby %}
bundle exec jekyll serve
{% endhighlight %}

Visit local version in browser : 

{% highlight ruby %}
http://127.0.0.1:4000/
{% endhighlight %}
