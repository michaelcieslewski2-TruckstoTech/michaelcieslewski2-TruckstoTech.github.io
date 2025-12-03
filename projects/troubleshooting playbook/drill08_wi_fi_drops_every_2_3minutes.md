---
layout: page
title: "Troubleshooting Drill #8 – Wi-Fi Drops Every 2-3 Minutes (Single User)"
permalink: /projects/troubleshooting%20playbook/drill08/
---

🛜 Wi-Fi Drops Every 2–3 Minutes (Single User) — Troubleshooting Playbook

🧩 Symptom Pattern

## 1. Confirm the Pattern

Run a continuous ping:

ping -t <gateway_or_dc_IP>


If you see timeouts every 2–3 minutes, suspect:

 - Short DHCP lease

 - 802.1X reauthentication

- Wi-Fi adapter power saving

## 2. Check IP + DHCP Lease

Run:

ipconfig /all


Check the wireless adapter for:

 - IPv4

 - Gateway

 - DNS

 - DHCP Enabled

 - Lease Obtained / Expires

If lease duration is a few minutes, that’s the cause.

## 3. Check DHCP Scope (Server)

Verify:

 - Lease duration (should be hours)

 - Scope utilization

 - Recent changes

Fix:

 - Set lease to normal (ex: 8 hours)

If only one device has issues, continue with client-side checks.

## 4. Test Wired vs Wireless

Plug in Ethernet.

If wired is stable and Wi-Fi drops:

 - Wireless NIC issue

 - Driver issue

 - 802.1X issue

 - Power-saving issue

## 5. Event Viewer (Client)

Event Viewer → Windows Logs → System
Filter sources:

 - DHCP-Client

 - WLAN-AutoConfig

 - 802.1X / EAP

Look for:

 - DHCP renewal failures

 - Authentication failures

 - Disconnect reason codes

If failures line up with the 2–3 minute cycle → authentication or driver problem.

## 6. Fix the Wi-Fi Adapter
Update Driver

Device Manager → Network Adapters
Update the wireless driver.

Disable Power Saving

Wireless Adapter → Properties → Power Management
Uncheck:

 - Allow the computer to turn off this device

Verify 802.1X (if used)

Check:

 - Certificates not expired

 - Trust chain present

 - Correct EAP/PEAP settings

## 7. Retest

Run:

ping -t <gateway_or_dc_IP>


Test for 5–10 minutes.
Look for:

 - No timeouts

 - Stable connection

 - Clean Event Viewer logs

## Root Cause (from drill)

Outdated wireless driver → 802.1X reauthentication failures → DHCP renewal fails → Wi-Fi drops every 2–3 minutes.
