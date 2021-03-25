---
layout: post
title: "Setting up personal blog on github pages"
date: 2021-03-21 13:32:20 +0300
description: Setting up personal blog on github pages. Jekyll , github pages, utteranc.es , utteranc , comments, issues
img:  # Add image post (optional)
---
As many of people who attempted and continued to develop, I've tried starting up my blog around 10+ times :) 
I tried many different options : 
 - Wordpress
-  some weird in place CMS
- adobe muse/ adobe webflow
- wix 
- Angular 1 static sites
- Angular 2 + Wordpress API
-  Static html pages
- Contenful CMS with some frameworks and etc. 

I wasn't really happy with those. From one side, tons of control, but do we actually need all of the overhead?
Damn no!

So, this is my final attempt which gonna live forever on Github Pages and doesnt really require nothing but tiny time investment to maintain it & set it up.

My requirment : 
- no hustle with servers or hosting
- super simple setup
- none or very little maintenance
- allows comments
- allows code formatting
- something used by many, and it proven by time

Result : blog on github pages

How did I compelete it : 

- used the template https://github.com/artemsheludko/flexible-jekyll
- updated the _config.yaml file with my own settings, uploaded my own profile pic to assets/img folder
- removed unnecessary posts in _post folder
- removed disqus integration from file _layouts/post.html 
Maybe for someone it works, I do find it too bulky...) 
- added integration with https://utteranc.es/ 
This one, adds to each post a github issue & allows people to comment via their gihub profile. Wich i find amazing simple & all stored in github :) 
