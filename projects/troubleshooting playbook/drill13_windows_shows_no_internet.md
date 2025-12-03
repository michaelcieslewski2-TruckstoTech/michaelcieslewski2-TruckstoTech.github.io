---
layout: page
title: "Troubleshooting Drill #13 - Windows Shows No Internet but Web Browsing Works"
permalink: /projects/troubleshooting%20playbook/drill13/
---

Windows Shows “No Internet” but Web Browsing Works
## Problem Summary
A user reports that Windows shows “No Internet” with a yellow triangle in the taskbar, even though they can browse normal websites. At the same time, Microsoft cloud apps (Teams, OneDrive, Outlook) fail to sign in and keep asking for a password. Other users in the office are fine. This typically points to DNS or proxy issues affecting Microsoft’s login endpoints rather than general network connectivity.

## Check Network Configuration (ipconfig /all)
Run:
ipconfig /all
Verify:
 - Valid IPv4 address (not APIPA 169.254.x.x)
 - Correct subnet mask
 - Correct default gateway
 - DNS servers match known-good machines
 - No unusual DNS suffixes
 - DHCP enabled (if normal for environment)
If everything looks normal, basic connectivity is fine. Continue deeper.

## Check DNS Resolution for Microsoft Login

Run:
nslookup login.microsoftonline.com
If the result shows:
Address: 127.0.0.1
This is a major red flag.
Why:
 - login.microsoftonline.com should never resolve to 127.0.0.1
 - 127.0.0.1 is the local machine
 - This breaks:
  - Teams sign-in
  - OneDrive sign-in
  - Outlook authentication
  - Windows connectivity tests (NCSI)
Conclusion: DNS for Microsoft login is being redirected locally, often due to proxy or security software.

## Check Proxy Settings (LAN Settings)
Open:
Control Panel → Internet Options → Connections tab → LAN Settings
Look for:
- “Use a proxy server for your LAN” → Checked
- Proxy address set to 127.0.0.1 or other loopback address
If so, Windows is routing all traffic through a local proxy.
If that proxy isn’t actually running or is misconfigured, Microsoft cloud apps will fail even while normal browsing works.
Fix:
 - Uncheck Use a proxy server for your LAN
 - Leave Automatically detect settings enabled
 - Click OK → Apply → OK
This removes the forced proxy and restores normal connectivity.

## Re-Test Microsoft Services
Teams
Open Teams → should now sign in normally.
OneDrive
Click the cloud icon → should stop showing sign-in errors and begin syncing.
Outlook
Open Outlook and check bottom-right:
 - “Disconnected” →
 - “Trying to connect…” →
 - “Connected to: Microsoft Exchange”
Mail should begin syncing.
Windows “No Internet” Indicator
The yellow triangle should disappear once Microsoft’s NCSI tests start succeeding again.

## Final Result
 - Windows now shows Internet access
 - Teams signs in
 - OneDrive syncs
 - Outlook connects and stays signed in
 - No more password prompts
 - Normal browsing still works

## Root Cause Summary

A proxy was manually enabled in LAN settings:
Address: 127.0.0.1
Port: 3128
This caused:
 - Microsoft login traffic to route to the local machine
 - DNS for login.microsoftonline.com to resolve incorrectly
 - NCSI tests to fail → “No Internet” indicator
 - All Microsoft apps to break
Fix was to disable the proxy in Internet Options.

## Key Lessons
1. If browsing works but Microsoft apps don’t, think:
 - Proxy
 - DNS redirection
 - SSL interception
 - NCSI failure
2. If nslookup login.microsoftonline.com resolves to 127.0.0.1, something is hijacking DNS.
3. Windows “No Internet” does not mean the user has no internet.
4. It means Microsoft’s connectivity tests failed.

Proxy misconfigurations are extremely common.

Old VPNs, dev tools, or security agents often leave invalid proxy entries behind.
