---
layout: page
title: "# Troubleshooting Drill #3 – Outlook Stuck on “Loading Profile”"
permalink: /projects/troubleshooting%20playbook/drill03/
---

 
**Difficulty:** Medium–Hard  

## Scenario  
Outlook hangs forever on launch.

## Steps Taken  
1. OWA works → mailbox/server good.  
2. Killed hung Outlook processes.  
3. Profile repair fails.  
4. Safe Mode fails.  
5. Autodiscover cache reset fails.  
6. Renamed OST → Outlook rebuilds → works.

## Root Cause  
Corrupted OST file.

## Lesson Learned  
OWA vs Outlook desktop instantly isolates profile/cache corruption.

