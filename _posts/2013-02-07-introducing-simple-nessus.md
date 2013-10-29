---
layout: post
title: Introducing Simple Nessus
tags: [script, project]
category: posts
---

**Simple Nessus** is a Perl script developed to simplify the output of the [Tenable Nessus][nessus] scanner by keeping only relevant information. It works like this:

	perl simple-nessus.pl {NESSUS-FILE} {VERSION} [SEVERITY] [OUTPUT] [OTHER-OPTIONS]
	
The version option can either be `-v1` or `-v2` depending on how the report was exported from Nessus.

You can additionally set the minimum vulnerability severity as following (default `-s L`):

	-s L:	low, medium, high and critical
	-s M:	medium, high and critical
	-s H:	high and critical

Furthermore, Simple Nessus provides several output options to meet your needs.  
The default `-o O` prints to STDOUT and, just like text output (`-o T`), it looks like:

	[*] 192.168.229.2
	Apache HTTP Server Byte Range DoS
	Apache Banner Linux Distribution Disclosure
	
The CSV output (`-o C`) looks like:

	host;vulnerability
	192.168.229.2;Apache HTTP Server Byte Range DoS
	192.168.229.2;Apache Banner Linux Distribution Disclosure
	
While the Markdown code, triggered by `-o M`, looks like:

	### 192.168.229.2
	* Apache HTTP Server Byte Range DoS
	* Apache Banner Linux Distribution Disclosure

Finally, the `-ports` option will show the specific ports for each vulnerability found by the scanner.

--

**Get Simple Nessus on [GitHub][simple-nessus]!**  
Have any suggestions to improve Simple Nessus? Contact me!


[nessus]: http://www.tenable.com/products/nessus
[simple-nessus]: http://gcattani.github.com/simple-nessus