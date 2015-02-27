---
layout: post
title: Detecting Chrome Extensions in 2013
tags: [researching]
category: posts
---

Detecting Google Chrome extension has always been a trivial job, as highlighted by [previous][scribe] [researches][kotowicz]. It was all a matter of finding the extension ID and to try loading its *manifest.json* file, which is always located in the extension's root directory:

	chrome-extension://abcdefghijklmnopqrstuvwxyz0123456789/manifest.json

However, in the latest version of Chrome, some attempts at loading the manifest were greeted with an unpleasent message:

> Denying load of chrome-extension://abcdefghijklmnopqrstuvwxyz0123456789/manifest.json. Resources must be listed in the web\_accessible_resources manifest key in order to be loaded by pages outside the extension.
	
What the heck is the *web_accessible_resources manifest key*? A quick look at the manifest file highlights the following array:
	
	"web_accessible_resources": [ "logo.png", "menu.html", "style.css" ]

Since Chrome 18, Google deprecated manifest version 1 and its support will be decreasing, making the old version unavailable in September 2013. Among the differences between [version 1 and 2][manifest], Google introduced the [web_accessible_resources][webaccres] array, which determines the local resources that can be loaded by external sources.

So, if the manifest file doesn't include it (ie. it's still version 1), any resource in the extension's directory can be loaded by external pages. Otherwise, if the file uses already the newer version (shown as `"manifest_version": 2`) and includes a *web_accessible_resources* array, only the listed resources can be loaded.

A generic JavaScript approach, which works for text files and pictures alike, builds a `script` element and sets its source to the desired file. The following code is just an example:

{% highlight javascript %}
var scriptSrc = document.createElement("script");			
scriptSrc.setAttribute("onload", "[ONLOAD CODE]");
scriptSrc.setAttribute("src", "chrome-extension://" + resource);
document.body.appendChild(scriptSrc);
{% endhighlight %}

While the previous code works for every type of file, an approach specific to picture files can be used if needed. The following code creates an `img` element:

{% highlight javascript %}
var imgSrc = document.createElement("img");
imgSrc.setAttribute("border", "0");
imgSrc.setAttribute("width", "0");
imgSrc.setAttribute("height", "0");
imgSrc.setAttribute("onload", "[ONLOAD CODE]");
imgSrc.setAttribute("src", "chrome-extension://" + resource);
document.body.appendChild(imgSrc);
{% endhighlight %}
	
While probably requiring updates to existing detection code, *web_accessible_resources* won't stop specific loading attempts, making extension detection still possible.


[scribe]: http://www.skeletonscribe.net/2011/07/sparse-bruteforce-addon-scanner.html
[kotowicz]: http://blog.kotowicz.net/2012/02/intro-to-chrome-addons-hacking.html
[manifest]: https://developer.chrome.com/dev/extensions/manifestVersion.html#manifest-v1-changes
[webaccres]: https://developer.chrome.com/dev/extensions/manifest.html#web_accessible_resources
