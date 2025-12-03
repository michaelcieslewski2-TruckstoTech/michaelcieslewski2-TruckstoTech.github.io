---
layout: page
title: "Troubleshooting Drill #12 - Outlook Shows Disconnected (Desktop App)"
permalink: /projects/troubleshooting%20playbook/drill12/
---

Outlook Shows “Disconnected” (Desktop App)

## Problem Summary

A user reports that Outlook shows “Disconnected” in the bottom-right corner. They cannot send or receive email, but they can browse the internet normally and OWA (webmail) works. Other users in the office have no issues. This indicates the problem is local to the Outlook desktop app, not the mailbox or the server.

## Confirm OWA Functionality

Ask the user to open:
https://outlook.office.com
If OWA loads and the user can send/receive mail, then:
 - The mailbox is healthy
 - Credentials are good
 - Exchange Online is reachable
This confirms the desktop app is the issue.

## Check for Simple Causes
Check “Work Offline”
Outlook → Send/Receive → Work Offline
 - If enabled, disable it
 - If already off, continue
Restart Outlook
Fully close Outlook → reopen.
If still disconnected, continue.

## Verify Account Status in Outlook
Go to:
File → Account Settings → Account Settings → Email tab
Check:
- Does the account show “Connected” or “Not Connected”?
- No “Needs Password” messages
- No warnings
If still “Not Connected,” continue.

## Clear Cached Credentials (Credential Manager)
1. Open:
Control Panel → Credential Manager → Windows Credentials
2. Remove entries related to:
 - Office16
 - MicrosoftOffice16_Data:SSPI
 - Outlook:email@company.com
 - ADFS
 - MicrosoftAccount
3. Restart Outlook
4. Re-enter credentials if prompted
If Outlook still shows “Disconnected,” continue.

## Start Outlook in Safe Mode

Run:
outlook.exe /safe
If still disconnected in Safe Mode, add-ins are not the cause → continue.

## Check Windows Proxy Settings (Critical Step)

This is one of the most common causes for Outlook staying “Disconnected.”
Steps:
1. Open Control Panel → Internet Options
2. Go to the Connections tab
3. Click LAN Settings
Look for:
 - “Use a proxy server for your LAN” → checked
 - Especially suspicious if set to:
  - 127.0.0.1:8080 or any loopback address
Fix:
 - Uncheck: Use a proxy server for your LAN
 - Leave: Automatically detect settings checked
 - Click OK
Why this matters:
Outlook will not connect to Exchange Online through an invalid proxy, even if web browsing works.

## Restart Outlook

After disabling the proxy, open Outlook again.
The status should go from:
 - “Disconnected”
to:
 - “Trying to connect…”
 - “Connected to: Microsoft Exchange”
Emails start syncing.
Issue resolved.

## Root Cause Summary

A system-wide proxy server was enabled in Windows LAN settings, pointing to 127.0.0.1:8080. This blocked Outlook from connecting to Exchange Online. Disabling the proxy resolved the issue.

## Key Lessons

If Outlook is “Disconnected” but OWA works, this usually points to:
 - Local profile/token issue
 - Proxy misconfiguration
 - Autodiscover failure
 - Corrupt OST
 - Bad credentials
Checking proxy settings is one of the highest-value steps.
Rebuilding the Outlook profile should be a last resort, not the first thing you try.
