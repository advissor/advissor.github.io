---
layout: post
title: "Deploying Nginx Helm chart on minikube using terraform"
date: 2021-05-04 13:32:20 +0300
description: Deploying Nginx Helm chart on minikube using terraform
img:  # Add image post (optional)
---

##  How to  runn Nginx deployment in Minikube with Terraform in own namespace

Prerequisites : 

- minikube
- terraform 
- helm 3

### What we will complete by the end of this exercise : 
 
- Install **Nginx Helm Chart**
- Set **values of Helm chart via Terraform** to use **NodePort** service and specific **Port**
- Use **default** namespace
- Access Nginx Deployment via minikube ip & NodePort port

**Nodeport** exposes the service on every node on a fixed port. 
In our case, will expose it on our minikube ip and fixed , dedicated port

Which makes it easy to access in combination with minikube ip

1) Start minikube : 

{% highlight ruby %}
minikube start
{% endhighlight %}

2) Clone this article [GIT REPO](https://github.com/advissor/terraform-minikube-helm-istio) 

Switch into correct folder : 

{% highlight javascript %}
cd helm-provider
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

6) Verify helm chart is installed

{% highlight ruby %}
helm ls
{% endhighlight %}


Should show : 
{% highlight ruby %}
➜  advissor.github.io git:(main) ✗ helm ls
NAME                    	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART                         	APP VERSION
nginx-ingress-controller	default  	1       	2021-05-04 23:56:51.256473 +0500 +05	deployed	nginx-ingress-controller-7.6.6	0.46.0     
{% endhighlight %}

6) Verify Nginx pods are created

{% highlight ruby %}
kubectl get pods
{% endhighlight %}

Expected output :
{% highlight ruby %}

NAME                                                       READY   STATUS    RESTARTS   AGE
nginx-ingress-controller-64658d6659-drmzd                  1/1     Running   0          114s
nginx-ingress-controller-default-backend-8cc5f7b5d-94658   1/1     Running   0          114s
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
kubectl get svc
{% endhighlight %}

{% highlight ruby %}
NAME            TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes                                 ClusterIP   10.96.0.1        <none>        443/TCP                      55d
nginx-ingress-controller                   NodePort    10.103.151.203   <none>        80:30201/TCP,443:32767/TCP   3m27s
nginx-ingress-controller-default-backend   ClusterIP   10.99.218.58     <none>        80/TCP                       3m27s
{% endhighlight %}

Use the port , from the output and "minikube ip"

{% highlight ruby %}
➜ minikube ip
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