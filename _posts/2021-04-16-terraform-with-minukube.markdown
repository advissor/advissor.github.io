---
layout: post
title: "Terraform kubernetes provider on minikube. Creating kubernetes namespaes"
date: 2021-03-26 13:32:20 +0300
description: Terraform kubernetes provider on minikube. Creating kubernetes namespaes
img:  # Add image post (optional)
---
Prerequisites : 

- terraform 
- minikube


1) Start minikube : 

{% highlight ruby %}
minikube start
{% endhighlight %}

2) Clone this article GIT repo : 

[Clone](https://github.com/advissor/terraform-minikube-helm-istio)

cd namespaces

Edit main.tf file, and edit appropiately to point to your kubeconfig file location :

{% highlight javascript %}
provider "kubernetes" {
  config_context_cluster   = "minikube"
  config_path = "~/.kube/config"
}
{% endhighlight %}


3) Initialize terraform 

{% highlight ruby %}
terraform init
{% endhighlight %}


4) Plan 

{% highlight ruby %}
terraform plan
{% endhighlight %}

5) Terraform apply 

{% highlight ruby %}
terraform apply
{% endhighlight %}


6) Verify Namespaces get created 

{% highlight ruby %}
kubectl get ns
{% endhighlight %}


Should show : 
{% highlight ruby %}
NAME                     STATUS   AGE
applications-namespace   Active   6s
monitoring-namespace     Active   6s
default                  Active   37d
kube-node-lease          Active   37d
kube-public              Active   37d
kube-system              Active   37d
{% endhighlight %}
