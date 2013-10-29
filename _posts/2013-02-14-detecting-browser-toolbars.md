---
layout: post
title: Detecting Browser Toolbars
tags: [research]
category: posts
---

In January 2013 I began my involvement with the [Browser Exploitation Framework][beef] (BeEF) and the first assignement was the development of a toolbar detection module. Since a preliminary web search highlighted the lack of widespread information about the matter, I got right into hands-on testing.

The module focused on the most common toolbars on the market: *Alexa*, *Answers.com*, *Ask*, *Bing*, *Google*, *Stumble Upon* and *Yahoo!*.

A first analysis uncovered that all the toolbars, with the only exception of the ones from Ask and Yahoo!, added a string to the browser User-Agent, thus making the detection trivial (i.e. `navigator.userAgent`):

* `Alexa Toolbar` (Alexa)
* `AskTbS-PV/5.15.12.33066` (Ask)
* `BRI/2` (Bing)
* `GTB7.4` (Google)
* `SU 3.96` (Stumble Upon)

A basic regular expression could then be used to detect which toolbars are installed and could be expanded later on to detect even more.

The User-Agent didn't provide any data to easily detect the Answers and Yahoo toolbars, so I moved onto DOM analysis, also to improve the overall reliability of the script. Unfortunately, only the Alexa toolbar seemed to add anything to the DOM, while there was no visible content added by the other software. The code that Alexa's toolbar added to the document head was the following:

	<script id="AlexaCustomScriptId"></script>
	
To my dismay, there was still no sign of the Answers and Yahoo toolbars anywhere! In the end, I pushed the module in its hybrid state, checking both DOM and User-Agent, and without being able to detect the two missing toolbars.


[beef]: http://beefproject.org