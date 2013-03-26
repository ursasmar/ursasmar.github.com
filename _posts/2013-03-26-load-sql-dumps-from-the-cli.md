---
layout: post
title: "Load SQL dumps from the cli"
description: ""
category: 
tags: []
---
{% include JB/setup %}

This is mainly a note for myself, but might be useful for others.

I often have to import bzipped dumps of mysql data into remote servers.  It is possible to use phpMyAdmin for this, but that can be a pain in the ass.  It's easier just to run it from to command line.

So, to get to the point,

{% highlight bash %}
bzcat ./path/to/data.dump.sql.bz2 | mysql --user=user --password=password database
{% endhighlight %}

If the file is gzipped, 

{% highlight bash %}
zcat ./path/to/data.dump.sql.gz | mysql --user=user --password=password database
{% endhighlight %}

Simple, and quick.
