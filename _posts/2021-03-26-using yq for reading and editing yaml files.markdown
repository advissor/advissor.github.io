---
layout: post
title: "Using YQ for reading and editing YAML files"
date: 2021-03-21 13:32:20 +0300
description:  Using YQ for reading and editing YAML files
img:  # Add image post (optional)
---

[YQ](https://mikefarah.gitbook.io/yq/)


Where this might come handy? GitOps !

In cases like updating values.yaml files when using Helm charts.
Updating docker image versions in CI/CD pipelines , to trigger changes & sync in K8S tools.
Editing any other file yaml, which carry versions and etc.


Lets take sample YAML file , called "test.yaml" with following content :

{% highlight ruby %}
# An employee record
name: Martin D'vloper
job: Developer
skill: Elite
employed: True
foods:
  - Apple
  - Orange
  - Strawberry
  - Mango
{% endhighlight %}


Lets select "martin" node 
{% highlight ruby %}
yq read test.yaml name 
{% endhighlight %}
Output 

{% highlight ruby %}
Martin D'vloper
{% endhighlight %}

Now lets try to edit "name" field within YAML file with YQ  :

{% highlight ruby %}
yq w -i test.yaml name "Chris OK"
{% endhighlight %}

{% highlight ruby %}
# An employee record
name: Chris OK
job: Developer
skill: Elite
employed: True
foods:
  - Apple
  - Orange
  - Strawberry
  - Mango
  {% endhighlight %}