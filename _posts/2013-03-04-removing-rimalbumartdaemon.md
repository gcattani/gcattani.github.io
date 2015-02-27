---
layout: post
title: Removing RimAlbumArtDaemon
tags: [tips]
category: posts
---

If you are on OS X and happen to use BlackBerry Desktop Software, you might have met the infamous *RimAlbumArtDaemon* process. This extremely unhelpful process is well-known to hog your memory for no apparent reason and to keep iTunes from quitting.

After struggling for days, I finally found a [solution][post], which I will report here for my own reference (hopefully helping others). 

{% highlight bash %}
$ sudo rm /Library/Application\ Support/BlackBerry/RimAlbumArtDaemon
{% endhighlight %}
	
I didn't found any reason against deleting the file completely, but if you'd like to play it safe you could just rename it.

{% highlight bash %}
$ cd /Library/Application\ Support/BlackBerry/
$ sudo mv RimAlbumArtDaemon RimAlbumArtDaemon.old
{% endhighlight %}

Note that the process will come back every time the BlackBerry software gets updated.


[post]: http://supportforums.blackberry.com/t5/Desktop-Software-for-Mac/blackberry-daemon-process-help/m-p/1403387#M7835
