---
layout: post
title: clear up cache of spotify for osx
date: 'Tue Jan 24 01:15:52 EET 2017'
categories: jekyll update
published: true
---


{% highlight ruby %}
find $HOME/Library/Caches/com.spotify.client/Data -type f -name "*.*" -size +1k -exec echo {} \; -exec rm -rf {} \;
{% endhighlight %}


usefully tool "ncdu" for all disk space.

#other way verbose rm -rfv

also you can add on cronjob
{% highlight ruby %}
➜  ~ crontab -l
#forspotify
MAILTO="mail@tuncayuysal.com"
0 22 * * 1-5 find $HOME/Library/Caches/com.spotify.client/Data -type f -name "*.*" -size +1k -exec echo {} \; -exec rm -rf {} \; > 1>/dev/null
{% endhighlight %}

