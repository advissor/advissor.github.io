---
layout: post
title: "Switching between different/multiple terraform versions"
date: 2022-06-06 13:32:20 +0300
description: "Switching between different/multiple terraform versions"
img:  # Add image post (optional)
---

## Prerequisites   

This can be achieved with an OS tool called tfenv

Currently tfenv supports the following OSes

- Mac OS X (64bit)
- Linux ( 64bit and Arm )
- Windows (64bit) - only tested in git-bash - currently presumed failing due to symlink issues in git-bash


## Install tfenv via Homebrew for MacOS



{% highlight ruby %}
brew install tfenv
{% endhighlight %}
# How to use 

### Check if tfenv is properly installed

{% highlight ruby %}
tfenv version
{% endhighlight %}

Pick the terraform version you want and use the **tfenv install** command : 

### List remotes : 

{% highlight ruby %}
  tfenv list-remote
{% endhighlight %}


### As alternative, for checking available terraform versions you can check the list in here : 

  https://releases.hashicorp.com/terraform /

{% highlight ruby %}
tfenv install 0.15.1
{% endhighlight %}

### Use the terraform version you've just installed

{% highlight ruby %}
tfenv use 0.15.1
{% endhighlight %}

### Check that proper terraform version is used

{% highlight ruby %}
terraform --version
{% endhighlight %}


### List all currently installed terraform versions : 

{% highlight ruby %}
tfenv list
{% endhighlight %}



# For windows (git-bash): 

Check out tfenv into any path (here is ${HOME}/.tfenv)

{% highlight ruby %}
git clone https://github.com/tfutils/tfenv.git ~/.tfenv
{% endhighlight %}


### Add ~/.tfenv/bin to your $PATH any way you like

{% highlight ruby %}
$ echo 'export PATH="$HOME/.tfenv/bin:$PATH"' >> ~/.bash_profile
{% endhighlight %}


OR you can make symlinks for tfenv/bin/* scripts into a path that is already added to your $PATH (e.g. /usr/local/bin) OSX/Linux Only!

{% highlight ruby %}
ln -s ~/.tfenv/bin/* /usr/local/bin
{% endhighlight %}

On Ubuntu/Debian 

Touching /usr/local/bin might require sudo access, but you can create ${HOME}/bin or ${HOME}/.local/bin and on next login it will get added to the session $PATH or by running . ${HOME}/.profile it will get added to the current shell session's $PATH.


{% highlight ruby %}
mkdir -p ~/.local/bin/
. ~/.profile
ln -s ~/.tfenv/bin/* ~/.local/bin
which tfenv
{% endhighlight %}

### Links : 

https://github.com/tfutils/tfenv