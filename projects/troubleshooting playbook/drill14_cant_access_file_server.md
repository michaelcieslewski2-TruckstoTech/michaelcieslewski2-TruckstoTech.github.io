---
layout: page
title: "Troubleshooting Drill #14 - Can't Access \\FILE-SERVER, but Internet Works"
permalink: /projects/troubleshooting%20playbook/drill14/
---

Can’t Access \FILE-SERVER, but Internet Works
## Problem Summary
A user cannot access shared drives and receives “Windows cannot access \FILE-SERVER.” They can browse the internet normally and can ping external sites like Google, but pinging FILE-SERVER by name returns “could not find host.” Other users in the office can access the file server, so the server is healthy. This points to a DNS/name-resolution problem on the user’s workstation.

## Check Network & DNS Configuration (ipconfig /all)
Run:
ipconfig /all
Verify:
 - IP address, subnet, and gateway match other users
 - DHCP Enabled = Yes
 - DNS servers match the company’s internal DNS
 - No unusual DNS suffixes
If DNS servers show Google DNS (8.8.8.8 / 8.8.4.4) or other public DNS, this explains everything:
 - Internet browsing works → public DNS resolves external sites
 - Internal names like FILE-SERVER do not exist on Google DNS
 - File shares rely on internal DNS → broken
Conclusion: Client is using the wrong DNS servers.

## Fix the DNS Server Settings
Check a known-good machine
Run:
ipconfig /all
Example:
DNS Servers: 10.10.1.10
             10.10.1.11
These are the correct internal DNS servers.
Correct DNS on the broken machine
Option A — Use DHCP (recommended)
1. Control Panel → Network and Internet → Network Connections
2. Right-click Ethernet → Properties
3. IPv4 → Properties
4. Set to:
 - Obtain an IP address automatically
 - Obtain DNS server address automatically
5. Apply and close
6. Run:
ipconfig /release
ipconfig /renew

Option B — Manually set DNS
1. Same path: Ethernet → IPv4 → Properties
2. Set:
 - Preferred DNS: 10.10.1.10
 - Alternate DNS: 10.10.1.11
3. Apply and close

## Verify DNS is Working (Name Resolution)
Test with nslookup
Run:
nslookup FILE-SERVER
Expected:
Server:  dns1.corp.local
Address: 10.10.1.10
Name:    FILE-SERVER.corp.local
Address: 10.10.2.25
If the server resolves to an internal IP, DNS is fixed.

## Test Connectivity to the File Server
Run:
ping FILE-SERVER
Expected:
Reply from 10.10.2.25: bytes=32 time<1ms TTL=128
This confirms:
 - DNS is correct
 - Network path to the server works
 - No firewall or routing issues

## Test Access to the File Share

1. Press Windows + R
2. Enter:
\\FILE-SERVER
Expected:
 - Shared folders appear (e.g., Shared, Home, Dept-Files)
 - Mapped drives reconnect successfully

## Investigate Why DNS Was Wrong (Root Cause)

Fixing DNS gets the user working, but determine what caused the change.
Common causes:
 - User manually set DNS to Google (very common)
 - VPN software overwrote DNS and didn’t restore it
 - “Privacy” or “security” tools forcing 8.8.8.8
 - Old configuration from imaging
Example from this drill:
 - User had installed “Private Shield VPN,” which forced public DNS and didn’t reset it when closed
Document this as the root cause.

## Final Outcome

 - User can access \\FILE-SERVER
 - Mapped drives work
 - Internet still works
 - DNS now uses internal company servers

## Key Lessons for Your Playbook

1. If internet works but internal file server names do not resolve, suspect DNS on the client, not the server.
2. “Ping request could not find host” → DNS failure, not network failure.
3. ipconfig /all is the fastest way to spot:
 - Wrong DNS
 - Wrong gateway
 - Wrong subnet
4. Public DNS (8.8.8.8 / 1.1.1.1) does not work in AD environments.
5. Fix the configuration first, then determine why it changed (VPN, user action, software).
