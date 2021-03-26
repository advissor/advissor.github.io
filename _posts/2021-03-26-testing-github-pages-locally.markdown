---
layout: post
title: "Testing github pages locally"
date: 2021-03-26 13:32:20 +0300
description: Testing github pages locally
img:  # Add image post (optional)
---
[Install Ruby](https://jekyllrb.com/docs/installation/macos/)

In case, after reloading terminal gem command requires "sudo", you can try to reinstall Ruby in following maner :
https://github.com/rbenv/ruby-build/issues/1360
{% highlight ruby %}
RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl)" rbenv install  2.6.5
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
