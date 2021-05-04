---
layout: post
title: "Terraform kubernetes provider on minikube. Creating namespaces and running nginx deployment"
date: 2021-03-26 13:32:20 +0300
description: Terraform kubernetes provider on minikube. Creating namespaces and running nginx deployment
img:  # Add image post (optional)
---

##  How to  runn Nginx deployment in Minikube with Terraform in own namespace

Prerequisites : 

- terraform 
- minikube

### What we will complete by the end of this exercise : 
 
- Create **2 namespaces**
- Deploy **Nginx deployment**
- Create a **service** with type **NodePort** to access it 

**Nodeport** exposes the service on every node on a fixed port. 
In our case, will expose it on our minikube ip and fixed , dedicated port

Which makes it easy to access in combination with minikube ip

1) Start minikube : 

{% highlight ruby %}
minikube start
{% endhighlight %}

2) Clone this article GIT repo : 

[Clone](https://github.com/advissor/terraform-minikube-helm-istio)

{% highlight javascript %}
cd applications
{% endhighlight %}

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

Enter : Yes

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

6) Verify Nginx pods are created
{% highlight ruby %}
kubectl get po -n applications
{% endhighlight %}

Expected output : 
{% highlight ruby %}
➜  applications git:(main) 
NAME                                      READY   STATUS    RESTARTS   AGE
scalable-nginx-example-5fbb9989bf-9mz9w   1/1     Running   0          82s
scalable-nginx-example-5fbb9989bf-mbhvd   1/1     Running   0          82s
{% endhighlight %}

7) Access Nginx deployment via NodePort IP

{% highlight ruby %}
kubectl get svc -n applications
{% endhighlight %}

{% highlight ruby %}
NAME            TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
nginx-example   NodePort   10.100.32.98   <none>        80:30201/TCP   2m35s
{% endhighlight %}

Use the port , from the output and "minikube ip"

{% highlight ruby %}
➜  applications git:(main) minikube ip
192.168.64.2

{% endhighlight %}


Taking into account my output : 

Port : 30201

Minikube ip : 192.168.64.2

Nginx service will be accessible on : 

{% highlight ruby %}
192.168.64.2:30201
{% endhighlight %}

8) You will see "Welcome to nginx!" page