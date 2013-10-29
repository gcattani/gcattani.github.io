---
layout: post
title: A Quick Analysis of a Visa-MasterCard Phishing Page
tags: [phishing]
category: posts
---

A phishing e-mail claiming it was from Visa got through Gmail's filters, making its way to my inbox. I've seen similar messages in the last months and decided to analyze the malicious web site.

![Phishing Email](/images/2013-07-16-Visa-MasterCard-Phishing/01-email.png)

First of all, the message conveys a sense of urgency, both in its content and in its object ("*Your Credit Card has been Suspended . Please Update It !*"). This is a well-known tactic used to make sure that users won't pass by the e-mail without giving it much thought.

What I did find unusual, is that the phishing address is not hidden in any way. While the URL could easily fool an unexperienced user, it is pretty clear that the compromised domain is `holistica2000.com.ar` ("*El Portal holístico del Tercer Milenio*").

As a side note, during my first visits Firefox wasn't warning me about the malicious nature of the website, but got onto it in less than 8 hours from the original e-mail.

![Phishing Page](/images/2013-07-16-Visa-MasterCard-Phishing/02-site.png)

At a first sight, the phishing page looks extremely polished and professional, sporting a good English level. However, a quick analysis of the source code showed that the page looks so real because it's a direct copy a [MasterCard's one][mastercard]. The source code includes the following comment:

	<!-- Mirrored from www.mastercard.ca/education/credit-rating.html by HTTrack Website Copier/3.x [XR&CO'2010], Sat, 13 Oct 2012 12:45:21 GMT -->

The fraudster didn't even bother to clean the code for obvious clues like this, which still won't be noticed by the typical phishing victim. The source code analysis didn't reveal any malicious script, confirming that the web site has been created for the sole purpose of phishing users.

It's worth noting that all the links on the page do not work, effectively keeping the user inside the phishing environment. Furthermore, Visa's logo has been added to the original MasterCard's one, in order to extend the victim's pool (while the phishing e-mail only mentioned Visa).

The central element of the page is a very long form, which asks for many personal information about the victim, as well as information regarding the credit card.

![Phishing Module](/images/2013-07-16-Visa-MasterCard-Phishing/03-module.png)

When the form data gets sent to `/validation.php`, the victim is then redirected to `/verifiedbyvisa.html`.

This second phishing page asks for more information, including the victim's Social Security Number, the ATM code, the *3D* and *Verified by Visa* passwords. It's worth noting that the form will ask for a *Verified by Visa* code even if the victim previously selected a different card type.

![Phishing PINs](/images/2013-07-16-Visa-MasterCard-Phishing/04-pins.png)

The greedy fraudster doesn't stop here! After sending the additional information to `/validation2.php`, the victim is redirected one more time, this time to `/bank-inf.html`.

This last phishing page asks for authentication information regarding online services. The targeted ones are PayPal, Moneybookers, Liberty Reserve, Payza, Alertpay.

An interesting addition is the possibility to skip this step if the victim doesn't use any of the listed services, increasing the realism of the whole page.

![Phishing Online Services](/images/2013-07-16-Visa-MasterCard-Phishing/05-online.png)

When all the requested information is sent (this time to `/validation-bank.php`), the user is even greeted with a message congratulating him/her on the newly acquired security!

![Final Congratulations](/images/2013-07-16-Visa-MasterCard-Phishing/06-congratulations.png)


[mastercard]: www.mastercard.ca/education/credit-rating.html