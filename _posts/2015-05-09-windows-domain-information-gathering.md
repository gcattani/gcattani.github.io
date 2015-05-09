---
layout: post
title: Information Gathering in a Windows Domain
tags: [pentesting]
category: posts
---

Some penetration test may be performed from a domain-connected workstation or, at least, provide a working domain account. In these cases, the domain controller can be a powerful (and unaware) ally that is able to provide useful information to design further attacks.

The [`net user`][net-user] command provides a complete list of all existing domain users, both active and inactive.

	net user /domain

![net user list](/img/2015-05-09-windows-domain-information-gathering/net-user-domain.png)

The same command can also be employed to request additional details about any specific domain user. This may prove very useful, for example, when looking for a high-privileged user (e.g. Global Group *"Domain Admins"*) to perform a targeted attack against it.

	net user <USERNAME> /domain

![net user details](/img/2015-05-09-windows-domain-information-gathering/net-user-username-domain.png)

The [`net accounts`][net-accounts] command provides information concerning the active password security policy. This information allows the execution of more effective Password Brute Forcing and Guessing attacks, without risking the massive lock-out of domain accounts.

	net accounts

![net accounts](/img/2015-05-09-windows-domain-information-gathering/net-accounts.png)

In addition to user accounts, the [`net view`][net-view] command - used without parameters - displays a list of hosts in the current domain.

	net view

![net view](/img/2015-05-09-windows-domain-information-gathering/net-view.png)

It may also prove useful to find the current domain controller (i.e. the one used by the workstation). The following command can be used to get the name of the domain controller, which can then be associated to a specific IP address with `nslookup`.

	echo %logonserver%

![echo logonserver](/img/2015-05-09-windows-domain-information-gathering/echo-logonserver.png)

The [`net localgroup`][net-localgroup] command allows the listing of all groups with administrative privileges on the current computer. This information may be useful to perform targeted attacks aimed at gaining control of high-privileged accounts.

	net localgroup administrators

![net localgroup administrators](/img/2015-05-09-windows-domain-information-gathering/net-localgroup-administrators.png)

Besides the several `net *` commands, another powerful command-line utility is the [**Windows Management Instrumentation Command**][wmic] (WMIC).

Using the `/NODE` switch, it is possible to select one or more specific computers, identified either by their network names or IP addresses.

The following command provides the username and domain of the account currently connected to the specified computer. This may be useful when looking for the workstation used by a specific user.

*Note: WMIC provides a "messy" output, but it is possible to request only specific values (comma delimited) with the `GET` function.*

	wmic /node:<COMPUTER> computersystem get username

![wmic computersystem](/img/2015-05-09-windows-domain-information-gathering/wmic-computersystem.png)

The following command returns a list of all active processes on the specified computer.

	wmic /node:<COMPUTER> process get caption,processid

![wmic process](/img/2015-05-09-windows-domain-information-gathering/wmic-process.png)

The following command returns the IP addresses and hostnames for all the network interfaces configured on the specified computer.

	wmic /node:<COMPUTER> nicconfig get ipaddress,dnshostname

![wmic nicconfig](/img/2015-05-09-windows-domain-information-gathering/wmic-nicconfig.png)

WMIC provides [many options][wmic-queries] and it is strongly suggested to take a look at its documentation in order to find other useful commands.

--

*Thanks to [Alberto Volpatto][alberto] for providing some of the screenshots included in this post.*

[net-accounts]: 	https://technet.microsoft.com/en-us/library/bb490698.aspx
[net-localgroup]: 	https://technet.microsoft.com/en-us/library/bb490706.aspx
[net-user]: 		https://technet.microsoft.com/en-us/library/bb490718.aspx
[net-view]: 		https://technet.microsoft.com/en-us/library/bb490719.aspx

[wmic]: 		https://msdn.microsoft.com/en-us/library/aa394531%28v=vs.85%29.aspx
[wmic-queries]: 	http://blogs.technet.com/b/askperf/archive/2012/02/17/useful-wmic-queries.aspx

[alberto]: 		http://albertovolpatto.com


