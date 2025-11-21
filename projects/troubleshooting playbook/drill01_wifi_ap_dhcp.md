---
layout: page
title: "Drill 1 – Wi-Fi / AP DHCP Failure"
permalink: /projects/troubleshooting%20playbook/drill01/
---


  
**Difficulty:** Easy–Medium  

## Scenario  
User cannot access the internet; Wi-Fi says **“Connected, no internet.”**

## Environment  
- Windows 10 laptop  
- Corporate Wi-Fi  
- Other users unaffected  

## Steps Taken  
1. Ran `ipconfig /all` → APIPA 169.254 address.  
2. Tried `ipconfig /renew` → DHCP unreachable.  
3. Wired test works → DHCP server healthy.  
4. Tested another Wi-Fi client → also fails.  
5. Inspected AP’s switch port → uplink cable loose.  
6. Reseated cable → Wi-Fi restored.

## Root Cause  
Loose AP uplink cable preventing DHCP responses.

## Lesson Learned  
If Wi-Fi gets APIPA and wired works → **check the AP uplink.**

