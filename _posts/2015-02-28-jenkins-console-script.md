---
layout: post
title: Exploiting Jenkins Console Script
tags: [pentesting]
category: posts
---

During a recent Penetration Test, I found an unauthenticated instance of [Jenkins][jenkins], an open source continuous integration (CI) tool.

![Unauthenticated Jenkins Web Application](/img/2015-02-28-jenkins-console-script/Jenkins-No-Auth.png)

While browsing the web application for interesting information, the *Jenkins Console Script* ('http://jenkins/script') got my attention. This feature allows users to execute arbitrary [Groovy][groovy] scripts directly on the server and, with a little help from Stack Overflow, I quickly pulled together the following code to execute any command.

{% highlight groovy %}
def command = """whoami"""
def proc = command.execute()
proc.waitFor()

println "return code: ${ proc.exitValue()}"
println "stderr: ${proc.err.text}"
println "stdout: ${proc.in.text}"
{% endhighlight %}

Using the script to browse the server's file system, a file that included authentication credentials for an additional corporate application was identified. Access to this service allowed the disclosure of the whole source code of the customer's applications.

![Reading Credentials via Console Script](/img/2015-02-28-jenkins-console-script/Jenkins-Read-Credentials.png)

The default configuration of Jenkins does not perform any security check and does not enforce user authentication, allowing anyone to access the web application, configure Jenkins and its jobs, perform builds, etc.

As stated in the [Jenkins Best Practice][jenkins-best-practices], it is very important to *always secure Jenkins*, even when it is implemented for intranet use only (as demonstrated during this Penetration Test). If interested, take a look at the official [Securing Jenkins][jenkins-securing] article.


[jenkins]: http://jenkins-ci.org/
[groovy]: http://groovy.codehaus.org/
[gitlab]: https://about.gitlab.com/
[jenkins-best-practices]: https://wiki.jenkins-ci.org/display/JENKINS/Jenkins+Best+Practices
[jenkins-securing]: https://wiki.jenkins-ci.org/display/JENKINS/Securing+Jenkins