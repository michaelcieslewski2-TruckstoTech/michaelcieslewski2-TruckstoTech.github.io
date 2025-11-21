---
layout: page
title: "Drill 2"
permalink: /projects/troubleshooting%20playbook/drill02/
---


# Troubleshooting Drill #2 – Shared Drive / DNS Issue  
**Difficulty:** Medium  

## Scenario  
User cannot access mapped drive `S:` → `\\FILE-SRV01\Shared`.

## Steps Taken  
1. `ping FILE-SRV01` fails.  
2. `ping <server IP>` works.  
3. `ipconfig /all` shows DNS = 8.8.8.8.  
4. Reset DNS to internal servers.  
5. Hostname resolves → drive reconnects.

## Root Cause  
Client using **public DNS**, not internal DNS.

## Lesson Learned  
If IP works but hostname fails → it’s **DNS**.

