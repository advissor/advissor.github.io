---
layout: post
title: "AWS lambda api gateway deployment via AWS CDK"
date: 2022-09-19 13:32:20 +0300
description: "AWS Lambda api gateway deployment via AWS CDK"
img:  # Add image post (optional)
---

## Prerequisites   


- Linux environment( tested on MacOS M1 )
- Python 3.8
- CDK installed [Instructions](../Installing-AWS-CDK-on-Mac/)
- [Code](https://github.com/advissor/aws-lambda-api-gateway-deployment-via-cdk) for this reference example 

## Creating Virtual Environment for installing the Python dependencies 

{% highlight ruby %}

python3 -m venv .venv
{% endhighlight %}

## Installing the Python dependencies 

{% highlight ruby %}
pip install -r requirements-dev.txt
pip install -r requirements.txt
{% endhighlight %}

requirements-dev.txt file contains the PyTest
PyTest is being used for a simple Unit Test use case against our Infrastructure

## Initialising the CDK environment 
{% highlight ruby %}
cdk bootstrap
{% endhighlight %}


## Preview the preview of the compiled Cloudformation template ( baed on Python code)
{% highlight ruby %}
cdk synth
{% endhighlight %}

## Run PyTest tests
Run unit tests in tests/unit/test_cdk_multienv_setup_stack.py file : 
{% highlight ruby %}
pytest
{% endhighlight %}
This checks if API Gateway Method exists in compiled Cloudformation template & Lambda has Function name OrderGet and runtime of python 3.8


## Deploy the stack to your AWS account
{% highlight ruby %}
cdk deploy
{% endhighlight %}

After deployment you will see the Outputs section : 

{% highlight ruby %}
✨  Deployment time: 43.47s
Outputs:
cdk-multienv.orderapiEndpointE2C47C71 = https://exampleyteye.execute-api.us-east-1.amazonaws.com/prod/
{% endhighlight %}

In this output, the value of "cdk-multienv.orderapiEndpointE2C47C71" is the API Gateway Stage endpoint URL.

We have a "Get" method defined at path "/order" in cdk_multienv_setup/cdk_multienv_setup_stack.py

##  Testing the deployment
{% highlight ruby %}
curl https://exampleyteye.execute-api.us-east-1.amazonaws.com/prod/order
{% endhighlight %}


Succesful response would return : 
{% highlight ruby %}
Transaction: id-12314124124124
{% endhighlight %}


This sends the request to API Gateway Endpoint, which forwards it to our Lambda function

### Links : 
https://docs.pytest.org/en/7.1.x/
https://docs.aws.amazon.com/cdk/v2/guide/work-with-cdk-python.html