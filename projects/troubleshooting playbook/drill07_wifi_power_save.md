# Troubleshooting Drill #7 – Intermittent Wi-Fi Drops (Power Saving Issue)  
**Difficulty:** Very Spicy  

## Scenario  
Wi-Fi appears connected, but traffic drops every 5–10 minutes.

## Steps Taken  
1. User stable on other AP → problem isolated to location/laptop.  
2. `ping -t` shows intermittent drops.  
3. Issue worse when plugged in → power clue.  
4. Checked Wi-Fi adapter power management → “Allow to turn off device” enabled.  
5. Wireless adapter power settings set to **Maximum Performance**.  
6. Connectivity stable afterward.

## Root Cause  
Wi-Fi power-saving mode intermittently disabling the NIC.

## Lesson Learned  
Power settings can silently cripple Wi-Fi without showing disconnects.
