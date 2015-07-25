---
layout: post
title: Simple Nessus v2
tags: [developing]
category: posts
---

**Simple Nessus** is a script developed to simplify the output files of the [Tenable Nessus][nessus] vulnerability scanner by keeping only relevant information.

I quickly put it together back in 2013 out of the need to parse huge Nessus outputs in a sensible time. Even though it served me well over these years, I've been thinking about an update for quite some time. Here it is!


### What's New

Simple Nessus works pretty much as before, but some changes have been implemented in this brand new version.

First off, Simple Nessus is now developed in Ruby, as I've been slowly abandoning Perl for my day-to-day scripting needs. This doesn't make it necessarily better, but it's a transition I've been planning for some time.

The script also returns way more information about the target hosts and vulnerabilities than before. I felt that the previous version was *too* simple concerning this aspect.

Furthermore, the main - and so far only - output is CSV. In my opinion, the additional output options were not that useful and I chose to pick only the most convenient formats.


### How It Works

Simple Nessus is still dead easy to use. To easily get going you can execute it with the following command, which will extract all the vulnerabilities from low to critical.

	ruby simple-nessus.rb -f <NESSUS FILE>

The script allows more options concerning which issues to extract and the output configuration. You can get more information in the readme file or using its own help option (`-h`).


### What's Planned

There are still some features that I'm planning on adding, the main one being more output options, such as XLSX. I'll try to understand which formats can actually be of use and implement them in the future.

Another plan I've been thinking about is to make Simple Nessus as a proper Ruby gem. I'm still not sure I'll go this way, but it will probably be an interesting project to personally get better at Ruby.

As always, suggestions concerning additional features (with a *simple* mindset) are always welcome.

--

*Get Simple Nessus v2 on [GitHub][simple-nessus]!*


[nessus]: http://www.tenable.com/products/nessus
[simple-nessus]: https://github.com/gcattani/simple-nessus
