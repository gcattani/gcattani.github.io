---
layout: post
title: Introducing ListInt
tags: [script, project]
category: posts
---

Ever felt the need to quickly and painlessly generate a long list of numbers? **ListInt** can help!

--

**ListInt** is a simple Perl script that does only one thing: generating user-defined lists of integers. It can be executed like this:

	perl listint.pl {FROM} {TO} [OPTIONS]

*FROM* is the first integer of the list and *TO* is the last one. ListInt is smart enough to understand that if a users provides a starting value higher than the ending one, the expected result is a descending list (e.g. from 100 to 0).

To further extend its features, ListInt is able to print only even (`-e`) or odd (`-o`) numbers between the values provided as arguments. Finally, ListInt can add arbitrary strings as prefix (`-p`) or suffix (`-s`) for all the numbers listed as its output.

Take a look at a couple of simple examples and [give it a try][listint]!

	$ perl listint.pl 10 0 -o -p id_
	id_9
	id_7
	id_5
	id_3
	id_1

	$ perl listint.pl 10 0 -e -s 000
	10000
	8000
	6000
	4000
	2000
	0000


--

**Get ListInt on [GitHub][listint]!**  
Know how to make ListInt better? Get in touch or fork the repository!


[listint]: https://github.com/gcattani/listint