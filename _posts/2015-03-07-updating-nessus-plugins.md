---
layout: post
title: Updating Tenable Nessus Plugins
tags: [tips]
category: posts
---

_Since I always keep forgetting how to manually update the plugins for [Tenable Nessus][nessus], I'm posting this here so I can easily find it later._

To perform a manual update of the plugin feed it's necessary to execute the following command. Note that Nessus will not provide any progress status, but will warn the user when the feed download has been completed.

{% highlight bash %}
$ sudo /opt/nessus/sbin/nessus-update-plugins
Fetching the newest updates from nessus.org...
Done. The Nessus server will start processing these plugins within a minute
{% endhighlight %}

To check whether the plugins have been correctly updated or not, it's possible to check the feed version by simply reading a specific file, as shown below.

{% highlight bash %}
$ cat /opt/nessus/lib/nessus/plugins/plugin_feed_info.inc
PLUGIN_SET = "201502240415";
PLUGIN_FEED = "ProfessionalFeed (Direct)";
{% endhighlight %}

After the update command has been successfully executed, Nessus will begin initialising. This may take some time depending on how old the plugin feed was.

![Nessus Initialisation](/img/2015-03-07-updating-nessus-plugins/Nessus-Initialising.png)


[nessus]: http://www.tenable.com/products/nessus-vulnerability-scanner
