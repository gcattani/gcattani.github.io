---
layout: post
title: Repeating Requests in Burp Suite
tags: [tips]
category: posts
---

In [Burp Suite][burp], the tool usually employed to manually repeat requests is *Burp Repeater*. However, to automatically repeat the *same* request without including any fuzzing or attack payload, the tool for the job is **Burp Intruder**.

Burp Intruder allows repeating requests without any change by selecting *"Null payloads"* as payload type under the *"Payloads"* tab.

This specific payload allows to repeat the request until the user manually stops the attack by selecting the *"Continue indefinitely"* option. On the other hand, it also allows to repeat the request for a set number of times by selecting the *"Generate _____ payloads"* option.

After setting up the payload and number of requests, the attack can be started normally via the *"Start attack"* button.

![Burp Intruder with Null Payloads](/img/2015-04-18-burp-suite-repeating-requests/Burp-Intruder-Null-Payloads.png)

Burp Intruder also allows further control over the timing of its automatic requests.

In the *"Options"* tab, it provides additional settings for the *"Request Engine"*, such as the time between requests (which could be fixed or variable) and the number of threads used.
 
![Burp Intruder Request Engine Options](/img/2015-04-18-burp-suite-repeating-requests/Burp-Intruder-Request-Engine.png)

This features allow, for example, to automatically send a fixed request to a remote server every minute for an hour (but any frequency works).


[burp]: http://portswigger.net/burp/
