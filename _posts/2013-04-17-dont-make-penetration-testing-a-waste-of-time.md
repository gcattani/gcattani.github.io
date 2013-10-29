---
layout: post
title: Don't Make Penetration Testing a Waste of Time
tags: [pentest, rant]
category: posts
---

Recently I read an interesting article by Jim Bird titled [*"Penetration Testing Shouldn't be a Waste of Time"*][swreflections], which was written following another post by Rohit Sethi, [*"Debunking Myths: Penetration Testing is a Waste of Time"*][sdelements].

Since both articles focus only on the client's side of an assessment, a critical part has not been taken into account: **the tester**.

--

The role of a proper penetration tester goes beyond the mere *pwnage* of the target system, as an assessment should provide a client with the clear understanding of the system's vulnerabilities and of the data at risk.

It all starts during the kick-off meeting(s), before the actual hands-on testing. This is the place where a tester must clearly understand what the client is asking (not always trivial) and which targets are the most critical in order to make the most out of the limited testing time available.

When dealing with systems in a production environment (and it happens more frequently than I'd like to admit!), define beforehand if, how and when critical vulnerabilities should be disclosed to the client, even meanwhile the test is still ongoing.

During the testing activities, ask for the client's assistance when needed: don't lose a whole day when a 5 minute call would have answered your doubts. If you struggle to understand how a process works, just ask. If you need your IP address to be whitelisted on the firewall in order to test properly, just ask (unless this is the scope of the assessment).

In the final report, be as clear as possible about the vulnerabilities found and the performed attacks, focusing on their root causes and their impact on the target application. Even simple screenshots should be clear and descriptions should get right to the point, without confusing the reader with unnecessary technical jargon or convoluted explanations.

In order to provide developers with enough information to properly solve any issue, make sure to include proofs of concept that can be easily replicated by following simple steps. Feel free to include more technical examples if needed, but only after the basic one.

Provide a realistic picture of the target's exposure, focusing on the most critical issues and without exaggerating low impact vulnerabilities. Your expertise is paramount for a client who will need to define an action plan in order to solve the system's problems.

When presenting the results of the test to the client, you must keep in mind who are the people listening and adapt accordingly. Most of the times, the people involved on the client's side are not tech-savy, at least not as much as a tester would like.

Be ready to explain anything included in the report and consider carefully the knowledge of the people you are talking to. You may have to explain, for example, Cross-Site Scripting (or worse, Cross-Site Request Forgery) to people who have no idea how browsers, HTML or HTTP work.

If the client has you meet the developers that would be fixing the security issues, provide suggestions that will help solving the issue permanently, instead of directing them to solutions that work only for a specific situation.

In the end, a successful penetration test is defined by both the client's and the tester's behaviour and mindset, but the tester plays a key role in all the parts of the project. As the one who knows how the whole project works, the tester should assist the client whenever needed, providing enough information to make him/her understand the value of such assessments.


[swreflections]: http://swreflections.blogspot.it/2013/04/penetration-testing-shouldnt-be-waste.html
[sdelements]: http://blog.sdelements.com/is-penetration-testing-a-waste-of-time/