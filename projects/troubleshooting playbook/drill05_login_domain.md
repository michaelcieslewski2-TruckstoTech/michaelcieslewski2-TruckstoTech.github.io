---
layout: page
title: "Drill 5 - Password Incorrect (Local vs Domain Login)"
permalink: /projects/troubleshooting%20playbook/drill05/
---



# Troubleshooting Drill #5 – Password Incorrect (Local vs Domain Login)  
**Difficulty:** Spicy–Hard  

## Scenario  
User cannot log in; password “incorrect.”

## Steps Taken  
1. Verified other users can log in → AD fine.  
2. Login screen shows “Sign in to: This device.”  
3. Checked Ethernet cable → loose.  
4. Reseated → domain reachable.  
5. User logs in normally.

## Root Cause  
PC defaulted to **local login** due to no domain connection.

## Lesson Learned  
“Wrong password” often = **wrong login scope** (local vs domain).

