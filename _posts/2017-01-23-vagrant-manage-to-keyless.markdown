---
layout: post
title:  "vagrant manage keyless"
date:   Mon Jan 23 18:19:25 EET 2017
author: Tuncay UYSAL
categories: jekyll update
---

sudo visudo 


{% highlight ruby %}
# Cmnd alias
Cmnd_Alias VAGRANT_HOSTS = /bin/cp /Users/Tuncay/.vagrant.d/tmp/hosts.local /etc/hosts
# User privilege
%sudo ALL=(root) NOPASSWD: VAGRANT_HOSTS
{% endhighlight %}

_ vagrant up



#trick
=> for sudo style => alias _="sudo" add in your shell profile..