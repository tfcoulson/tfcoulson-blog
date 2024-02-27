---
layout: post
title: Golden Ticket Attacks using Impacket
---

After playing about with Mimikatz for a bit and looking at a few options to bypass the anti virus, I decided a different approach might make more sense.
Impacket contains a number of tools that can be used to obtain the krbtg hash and the domain SID, and then to create a golden ticket offline.

