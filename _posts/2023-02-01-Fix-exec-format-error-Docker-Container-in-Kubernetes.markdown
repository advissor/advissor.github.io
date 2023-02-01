---
layout: post
title: "Fixing 'exec format error' when Launching Docker Container in Kubernetes"
date: 2023-02-01 11:00:00 +0300
description: "Learn how to resolve the exec format error when launching a Docker container built on Mac in Kubernetes and how to build a multi-arch Docker image on Mac suitable for Linux Kubernetes."
img:  # Add image post (optional)
---

## Reason for the Error

The "exec format error" occurs because the binary format of the executable file inside the Docker container is not compatible with the architecture of the machine on which the container is being run. For example, if a Docker container built on a Mac is launched in a Kubernetes cluster running on Linux, the Linux machine may not be able to execute the binary file because it was built for a different architecture.

## How to Fix the Error


To resolve the "exec format error," you need to build a multi-arch Docker image that is suitable for both Mac and Linux. 

This will ensure that the binary format of the executable file inside the container is compatible with both Mac and Linux architectures.

Building a Multi-Arch Docker Image on Mac

{% highlight ruby %}
docker buildx build --push --platform linux/arm/v7,linux/arm64/v8,linux/amd64 -t <image-name> .
{% endhighlight %}


### Links : 

https://docs.docker.com/engine/reference/commandline/buildx_build/

https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/