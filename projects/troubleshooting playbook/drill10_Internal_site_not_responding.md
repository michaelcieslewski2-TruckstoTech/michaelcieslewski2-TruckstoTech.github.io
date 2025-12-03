---
layout: page
title: "Troubleshooting Drill #10 –  Internal Site Not Reslvong (intranet.corp.local)"
permalink: /projects/troubleshooting%20playbook/drill10/
---
Internal Site Not Resolving (intranet.corp.local)
Problem Summary

A user can reach internet sites (Google, etc.) but cannot access the internal site intranet.corp.local. The browser says the site cannot be reached, and pinging intranet.corp.local returns a “DNS name does not exist” error. Other users in the office can access the same internal site without problems. The goal is to determine why this single machine cannot resolve intranet.corp.local when others can.

## Recognize the Likely Problem Area: DNS

Because the user can reach internet sites, basic connectivity (NIC, gateway, routing) is probably fine.
The error message specifically says the DNS name does not exist, which strongly suggests a DNS/name-resolution issue rather than a server outage.

Mental note: “This smells like DNS, but I still need to prove it.”

## Use nslookup to Confirm the DNS Error

Run on the broken machine:

nslookup intranet.corp.local


Example output:

Server:  dns1.corp.local
Address: 10.10.1.10

*** dns1.corp.local can't find intranet.corp.local: Non-existent domain


What this tells you:

 - The client can reach the DNS server dns1.corp.local (10.10.1.10).

 - The DNS server is responding but says it cannot find that name.

 - The behavior matches the user’s report.

At this point, either:

 - DNS is broken globally for that name (unlikely, because others can reach it), or

 - Something about this client’s name resolution is misconfigured (more likely).

## Compare Network Settings with a Working Machine

Now answer: “What is different about this machine?”

On the broken machine

Run:

ipconfig /all


Key output:

Connection-specific DNS Suffix  . : corp.lcoal
DNS Servers . . . . . . . . . . . : 10.10.1.10

On a working machine

Run:

ipconfig /all


Key output:

Connection-specific DNS Suffix  . : corp.local
DNS Servers . . . . . . . . . . . : 10.10.1.10


Critical observation:

 - Broken: corp.lcoal

 - Working: corp.local

A tiny typo in the DNS suffix on the broken machine is causing lookups like intranet.corp.lcoal, which do not exist in DNS.

## Fix the DNS Suffix

We now know the root cause: incorrect DNS suffix on the broken machine.

Windows GUI path:

1. Right-click the network icon → Open Network & Internet settings.

2. Click Change adapter options.

3. Right-click the active Ethernet/Wi-Fi adapter → Properties.

4. Select Internet Protocol Version 4 (TCP/IPv4) → Properties.

5. Click Advanced… → go to the DNS tab.

6. Fix the suffix:

 - Change corp.lcoal to corp.local.

7. Click OK → OK → Close to apply.

(In a real environment this could also be fixed via DHCP or GPO, but for the drill you fix it locally.)

## Re-Test Name Resolution
Test with nslookup

Run:

nslookup intranet.corp.local


Expected output:

Server:  dns1.corp.local
Address: 10.10.1.10

Name:    intranet.corp.local
Address: 10.10.2.50


DNS now resolves the hostname to an IP address.

Test connectivity with ping

Run:

ping intranet.corp.local


Example output:

Pinging intranet.corp.local [10.10.2.50] with 32 bytes of data:
Reply from 10.10.2.50: bytes=32 time<1ms TTL=64


This confirms the network path to the server is good.

Test in the browser

Have the user browse to:

http://intranet.corp.local


(or https:// as appropriate)

Result:

 - Intranet page loads correctly.

 - Ticket resolved.

## Key Lessons from This Drill

1. Read the error messages carefully.
“DNS name does not exist” points to a DNS/name-resolution problem, not necessarily “server is down.”

2. Use nslookup to confirm DNS behavior.
It shows which DNS server you are using and what it returns.

3. Always compare with a known-good machine.
Running ipconfig /all on both broken and working clients is extremely powerful. Pay close attention to:

 - IP address and subnet

 - Default gateway

 - DNS servers

 - DNS suffix/search domain

4. Tiny typos can cause big problems.
corp.lcoal vs corp.local is easy to miss unless you slow down and scan carefully.

5. Generic flow for “internal site not resolving, others can reach it”:

 - Confirm basic connectivity (internet works, gateway reachable).

 - Run nslookup <hostname> on the broken machine.

 - Compare ipconfig /all with a working machine.

 - Fix any DNS misconfig (servers, suffix, static entries).

 - Re-test with nslookup, then ping, then a browser.
