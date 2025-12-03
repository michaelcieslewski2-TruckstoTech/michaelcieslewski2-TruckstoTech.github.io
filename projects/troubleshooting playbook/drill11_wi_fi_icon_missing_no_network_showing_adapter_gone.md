---
layout: page
title: "Troubleshooting Drill #11 – Wi-Fi Wi-Fi Icon Missing/ No Networks Showing/ Adapter Gone"
permalink: /projects/troubleshooting%20playbook/drill11/
---

Wi-Fi Icon Missing / No Networks Showing / Adapter Gone

## Problem Summary

A user reports that their laptop suddenly cannot connect to Wi-Fi. The Wi-Fi icon is missing from the taskbar, and in Network Settings the only options shown are Ethernet and VPN—no Wi-Fi at all. Other users in the office can connect normally, and no Ethernet cable is plugged in. This usually indicates the Wi-Fi radio or adapter on the client has been disabled, not a network-wide issue.

## Confirm the Symptoms

Ask the user what they see:

 - Is the Wi-Fi icon missing from the taskbar?

 - In Settings → Network & Internet, does it only show Ethernet and VPN, with no Wi-Fi section?

Ask:

 - “Can other people connect to Wi-Fi right now?”

If others can connect and the Wi-Fi section is missing, this almost always points to the laptop’s Wi-Fi radio/adapter being turned off or disabled.

## Check Airplane Mode

1. Press Windows + A to open Action Center.
2. Check Airplane mode:
 - If ON → turn it OFF.
3. After turning it off, check whether:
 - The Wi-Fi icon reappears
 - The Wi-Fi section shows up in Network Settings

If Airplane Mode was already OFF, continue.

## Check Device Manager for the Wi-Fi Adapter

Open Device Manager:
 - Right-click Start → Device Manager
 - Or Windows + X → Device Manager
Expand Network adapters. Look for a Wi-Fi entry such as:
 - Intel(R) Dual Band Wireless-AC…
 - Realtek Wireless LAN…
 - Anything with Wireless, Wi-Fi, or 802.11
Possible outcomes:
 - Adapter present, looks normal → may be disabled or a software issue
 - Adapter present with yellow icon → driver or hardware problem
 - Adapter missing entirely → very strong sign of:
      - Wi-Fi radio switched off (hardware/Fn key)
      - BIOS/firmware disabled wireless
      - Driver failure
      - Hardware failure

In this drill, the Wi-Fi adapter was missing from Device Manager.

## Check the Physical Wi-Fi Switch or Function Key

Many laptops have:
 - A physical wireless switch, or
 - A function key combo like Fn + F2, Fn + F3, etc., with a wireless icon

Steps:
1. Inspect the laptop for a Wi-Fi icon on the function keys or a physical switch.
2. Toggle the wireless control (Fn + Wi-Fi key).
3. Watch for a notification such as:
 - “Wireless capability enabled”
 - “Wi-Fi turned on”
4. Return to Device Manager → Network adapters.
Expected result:
 - A Wi-Fi adapter suddenly reappears with no warning icon.
 - In this drill, enabling the physical Wi-Fi radio caused the missing adapter to reappear immediately.

## Verify Wi-Fi Adapter in Network Settings

Open:
 - Settings → Network & Internet
Confirm:
 - A Wi-Fi section is now visible
 - Available networks are listed
Optional check:
 - Control Panel → Network and Sharing Center → Change adapter settings
 - Ensure the Wi-Fi adapter is Enabled, not greyed out.

Reconnect the User to Wi-Fi
1. Click the network icon in the taskbar.
2. Select the correct SSID.
3. Click Connect.
4. Enter the Wi-Fi password if needed.
5. Verify connectivity:
 - Browse to a known site (e.g., microsoft.com)
 - Optionally run: ping 8.8.8.8 or ping www.google.com
6. Ask the user to confirm they see the Wi-Fi icon again and can get online.

## Root Cause Summary

The laptop’s hardware Wi-Fi radio had been turned off using a physical switch or Fn key. Because of this:
 - The Wi-Fi icon disappeared from the taskbar
 - The Wi-Fi section vanished from Network Settings
 - The Wi-Fi adapter was hidden in Device Manager

Re-enabling the Wi-Fi radio brought the adapter back, restored the Wi-Fi settings, and allowed normal connectivity.

## Key Lessons

1. If Wi-Fi disappears completely (no icon, no adapter, no networks), think:
 - Airplane Mode
 - Physical Wi-Fi switch / Fn key
 - Device Manager
2. Start simple before digging into drivers or OS issues.
3. If Device Manager shows no Wi-Fi adapter, a disabled Wi-Fi radio is extremely common on laptops.
