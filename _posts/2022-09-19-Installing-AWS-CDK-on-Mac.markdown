---
layout: post
title: "Installing AWS CDK on Mac"
date: 2022-09-19 13:32:20 +0300
description: "Installing AWS CDK on Mac"
img:  # Add image post (optional)
---

## Prerequisites   

- Mac OS X (M1)
- Linux ( 64bit and Arm )
- Python 3 / NPM 

## Install tfenv via Homebrew for MacOS


Install or update the AWS CDK CLI from npm (requires Node.js ≥ 14).  
{% highlight ruby %}
npm i -g aws-cdk
{% endhighlight %}

Verify CDK is installed:

{% highlight ruby %}
cdk version
{% endhighlight %}

If installation is succesful should show similar to below : 

{% highlight ruby %}
2.32.1 (build 79cbe95)
****************************************************
*** Newer version of CDK is available [2.42.0]   ***
*** Upgrade recommended (npm install -g aws-cdk) ***
****************************************************
{% endhighlight %}


### Links : 

https://github.com/aws/aws-cdk