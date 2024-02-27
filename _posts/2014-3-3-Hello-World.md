---
layout: post
title: Golden Ticket Attacks using Impacket
---

After playing about with Mimikatz for a bit and looking at a few options to bypass the anti virus, I decided a different tool might make more sense.
Impacket contains a number of tools that can be used to obtain the krbtg hash and the domain SID, create a golden ticket offline and then obtain a shell using the Kerberos ticket.

Notes - credentials are needed to run these commands on the domain controller. These can be obtained by cracking NTLM hashes  or running a MITM6 attack.

