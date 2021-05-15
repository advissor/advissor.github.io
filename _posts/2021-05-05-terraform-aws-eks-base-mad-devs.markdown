---
layout: post
title: "Deploying EKS cluster using AWS EKS Base from Mad Devs"
date: 2021-05-05 13:32:20 +0300
description: Deploying EKS cluster using AWS EKS Base from Mad Devs
img:  # Add image post (optional)
---

##  Deploying EKS cluster using AWS EKS Base from Mad Devs via Terraform and Terragrunt

Prerequisites : 

- Terraform v0.15.x
- Terragrunt v0.29.x
- helm 3
- kubectl
- aws cli & aws admin creds

### What we will complete by the end of this exercise : 
 
- Configure local **Terraform** and **Terragrunt** setup
- Deploy the **AWS EKS cluster ** with VPC**, **OIDC provider**
- Deploy **HELM charts** for Prometheus, Grafana, Loki
- Out of the box logging and metric collection functionality

Helpful video from **Anton Babenko** stream **weekly.tf** :
Site with all weekly.tf streams : 


[Site](https://weekly.tf/) 

[Stream : AWS EKS and K8S with Terraform](https://www.youtube.com/watch?v=giVShrQHf8E&feature=youtu.be) 

What is being deployed into our EKS cluster 


![Taking from official repo https://github.com/maddevsio/aws-eks-base](https://raw.githubusercontent.com/maddevsio/aws-eks-base/main/docs/aws-base-diagrams-Namespaces-v3.svg)


Approximate infrastructure cost : 
{% highlight ruby %}
~216.8 dollars
{% endhighlight %}
1) Clone the  [repo](https://github.com/maddevsio/aws-eks-base#how-to-use-this-repo) and switch to "terraform" folder : 

{% highlight ruby %}
git clone https://github.com/maddevsio/aws-eks-base.git

{% endhighlight %}

2) Switch to terraform 0.15.1

{% highlight ruby %}

tfenv install 0.15.1
tfenv use 0.15.1
{% endhighlight %}

Make sure you also have Terragrunt v0.29.x , cause this is the lowest supported version by Terraform 0.15

3) Setup your aws creds 


{% highlight ruby %}
$ aws configure --profile yourProfileName
AWS Access Key ID [None]: *****************
AWS Secret Access Key [None]: *********************
Default region name [None]: us-east-1
Default output format [None]: json
{% endhighlight %}



4) Export AWS profile variable , with value of profile name configured higher :  

{% highlight ruby %}
$ export AWS_PROFILE=yourProfileName
{% endhighlight %}


5) Set up the domain name , which means adding hosted zone to your AWS account > Route 53 service

Record for future use the following :

{% highlight ruby %}
   Domain name 
   Hosten zone ID
{% endhighlight %}

**TIP** : recommend to add a dummy A record with value 8.8.8.8 undet the domain name (example : test.domain.com ), and try to perform **nslookup**  against it , from your PC. This will make sure your domain name is provisioned and working. 

The AWS ACM SSL certificate issueance used in the terraform module and some other parts, fully rely on the proper domain name configuration

If domain name configuration not properly done, you might have to wait for very long time, and then see an error.

So, I would recommend to spend some time on additional checks.

IF it goes well, you can proceed to step 4. 


6) Use **demo.tfvars.example** vars ,as a reference your own file with variables called **terraform.tfvars**

{% highlight ruby %}
cp terraform/layer1-aws/demo.tfvars.example terraform/layer1-aws/terraform.tfvars
{% endhighlight %}

7) Instuction of the eks base repo says you can remove the aws-sm-secrets file. 

Per my deployment attempt, that resulted in errors. **Please keep it and don't remove it**

So, what I did was , going with option 2 of creating an empty AWS Secret Manager secret with following naming convention :

In the root of layer2-k8s is the aws-sm-secrets.tf where several local variables expect AWS Secrets Manager secret with the pattern /${local.name}-${local.environment}/infra/layer2-k8s

{% highlight ruby %}
/${local.name}-${local.environment}/infra/layer2-k8s
{% endhighlight %}

I've inserted the dummay content :

{% highlight ruby %}
{
  "kibana_gitlab_client_id": "YYYYYYY",
  "kibana_gitlab_client_secret": "YYYYYYY",
  "kibana_gitlab_group": "YYYYYYY",
  "grafana_gitlab_client_id": "YYYYYYY",
  "grafana_gitlab_client_secret": "YYYYYYY",
  "gitlab_registration_token": "YYYYYYY",
  "grafana_gitlab_group": "YYYYYYY",
  "alertmanager_slack_url": "YYYYYYY",
  "alertmanager_slack_channel": "YYYYYYY"
}
{% endhighlight %}

6) Edit the  **terraform.tfvars** file and set following variables: 

File : **terraform/layer1-aws/terraform.tfvars**

{% highlight ruby %}
   create_acm_certificate = true
   zone_id = "Z07XXXXXXXXXXXx"
{% endhighlight %}

**create_acm_certificate** - stands for creating a SSL cert , which gonna be set on Load Balancer and mapped back to the Domain name.

**zone_id** - is the Hosted Zone ID , configurd in step **5**


7)  Set a remote state AWS S3 bucket name variable

{% highlight ruby %}
export TF_REMOTE_STATE_BUCKET=eks-base-demo-test
{% endhighlight %}

**TF_REMOTE_STATE_BUCKET** should be set to the desired S3 Bucket Name. Bare in mind,the names of S3 buckets are unique accross all AWS Customers.

8) Initialise the Terraform modules and state

Which will download all the needed Terraform modules, configured and try to create a remote state S3 bucket and save the state in there

Switch to **terraform** folder, so 2 layers are inside of it.

{% highlight ruby %}
terragrunt run-all init 
{% endhighlight %}  

9) Perform a Plan , is succesful try to perform an APPLY

{% highlight ruby %}
terragrunt run-all plan
terragrunt run-all apply
{% endhighlight %}  

10) To set the kubectl context, use the **aws eks update-kubeconfig** , shown in the outputs of the apply

Example : 

{% highlight ruby %}
aws eks update-kubeconfig --name eks-base-demo-use1 --region us-east-1
{% endhighlight %}  


11) If you want to access Grafana, retrieve the initial user password : 


Getting Creds to Grafana "admin" user : 

{% highlight ruby %}
kubectl get secret --namespace monitoring kube-prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
{% endhighlight %}  

12) Once finished with testing, if you want to destroy the stack , perform the following command : 

{% highlight ruby %}
terragrant run-all destroy
{% endhighlight %}  

