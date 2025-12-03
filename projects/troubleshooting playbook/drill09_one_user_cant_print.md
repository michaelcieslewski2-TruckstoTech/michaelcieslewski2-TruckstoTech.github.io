---
layout: page
title: "Troubleshooting Drill #9 – One User Can't Print to Shared Network Printer (Others Can)"
permalink: /projects/troubleshooting%20playbook/drill09/
---

 One User Can’t Print to a Shared Network Printer (Others Can)
Problem Summary

A single user cannot print to a shared network printer, even though the printer is online and other users can print normally. On this user’s PC, print jobs either stay stuck on "Printing…" or briefly appear in the queue and disappear with no output. Restarting the spooler or clearing the queue does not resolve the issue, meaning the problem is isolated to this workstation.

## 1. Check This User’s Print Queue

 - Devices & Printers → Right-click printer → See what’s printing

 - Clear all jobs

 - Try printing a test page

If the job hangs or disappears while others can print, the issue is specific to this machine.

## 2. Compare With a Working PC

On a known-good machine:

 - Confirm it prints successfully

 - Check Printer Properties → Ports and Driver

This shows the correct configuration.

## 3. Check the Printer Port on the Broken PC

Printer Properties → Ports

Verify:

 - It should use a Standard TCP/IP Port pointing to the printer’s IP

 - If it is using a WSD port, that is often the cause

Clue from this drill:
Working PCs used Standard TCP/IP; the broken one used WSD.

## 4. Fix the Printer Port

If WSD is selected:

 - Add Port

 - Choose Standard TCP/IP Port

 - Enter the printer’s IP

 - Finish the wizard

 - Select the new port

 - Apply / OK

Test print again — the job should process and the printer should respond.

## 5. If Still Broken: Driver Cleanup

If the port is correct but printing still fails:

1. Remove the printer

2. Open printmanagement.msc

3. Check Drivers for errors or partial installs

4. Remove the driver package

5. Reinstall the printer with the correct IP/port

Root Cause (From Drill)

The affected PC was using a WSD port instead of a Standard TCP/IP port, causing jobs to disappear even though the printer appeared online
Jobs disappeared because WSD failed even though printer showed “online”
