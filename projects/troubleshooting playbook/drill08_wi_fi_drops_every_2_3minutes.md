---
layout: page
title: "Troubleshooting Drill #8 – Wi-Fi Drops Every 2-3 Minutes (Single User)"
permalink: /projects/troubleshooting%20playbook/drill08/
---

🛜 Playbook: Wi-Fi Drops Every 2–3 Minutes (Single User)
🧩 Symptom Pattern
•	User on Wi-Fi only (wired is fine or not used)
•	Connection works for 1–3 minutes, then:
o	Drops, times out, or hangs
o	Then reconnects by itself
•	Other users on the same Wi-Fi are not having the issue
 
1. Confirm the Pattern
1.1. Run continuous ping
 
ping -t <domain_controller_or_gateway_IP>
•	Look for repeated “Request timed out” every few minutes.
•	If it happens on a pattern (every 2–3 min), think:
o	DHCP lease problems
o	802.1X reauthentication
o	Power-saving on Wi-Fi adapter
 
2. Check the IP & Lease
2.1. Run:
 
ipconfig /all
Note for the wireless adapter:
•	IPv4 address
•	Subnet mask
•	Default gateway
•	DNS servers
•	DHCP enabled: Yes/No
•	Lease Obtained / Lease Expires
🔑 Clue from this drill:
Lease duration was only 3 minutes, matching the drop pattern.
If lease is weirdly short (minutes instead of hours):
•	There might be a DHCP scope misconfiguration.
•	But that’s only part of the picture—other devices may still renew fine.
 
3. Check DHCP Scope (Server Side)
On the DHCP server, check the scope for that subnet:
•	Lease duration — should usually be hours, not minutes
•	Scope utilization — Is it almost full (e.g., 90%+)?
•	Any weird config changes recently? (intern, new tech, etc.)
Action:
•	Set lease duration back to normal (e.g., 8 hours)
•	Apply changes
•	This does not kick clients off; they’ll pick up new lease length on next renewal.
If other clients are fine and only this laptop is dropping, you still have a client-side issue.
 
4. Compare Wired vs Wireless
4.1. Plug in Ethernet and test:
•	Does it get a normal lease?
•	Is ping stable—no drops?
•	No weird DHCP or Auth errors?
If wired works fine and only Wi-Fi drops:
•	Problem is Wi-Fi specific:
o	Wireless NIC
o	802.1X/auth configuration
o	Driver
o	Power saving
 
5. Check Event Viewer for Wireless/Auth Issues
On the laptop:
Event Viewer → Windows Logs → System
Filter by sources like:
•	DHCP-Client
•	WLAN-AutoConfig
•	EAPOL or 802.1X/Authentication related
🔑 Clues from this drill:
•	DHCP-Client:
o	Event ID 1001: unable to renew address (timeout)
o	Event ID 1002: DHCPNACK
•	802.1X/EAP:
o	Authentication failed due to timeout
o	802.1X supplicant restarting authentication
•	WLAN:
o	Disconnect reason: authentication failure
If you see auth failures right before the drops, it’s almost always:
•	802.1X reauth problem
•	Wireless driver issue
•	Cert/time issue
 
6. Fix the Wi-Fi Side
6.1. Update Wireless Driver
•	Go to Device Manager → Network Adapters
•	Check wireless adapter version/date
•	Compare with standard/known-good version in your environment
•	Update to the approved/current driver
•	Reboot
6.2. Check Power Management (good habit)
In Device Manager → Wireless Adapter → Properties → Power Management:
•	Uncheck:
o	“Allow the computer to turn off this device to save power”
6.3. Verify 802.1X / Certificates (if using WPA2-Enterprise)
•	Ensure client/machine certificate is:
o	Present
o	Not expired
o	Trusted CA chain
•	Make sure system time is correct (if joined to a domain, usually okay).
 
7. Retest
•	Disconnect wired
•	Connect to Wi-Fi again
•	Run:
 
ping -t <DC_or_gateway>
•	Let it run 5–10 minutes
•	Confirm:
o	No drops
o	No repeating timeouts every 2–3 minutes
o	Event Viewer stays clean
✅ Root cause from this drill:
Outdated wireless driver → 802.1X reauth failures → DHCP renewal fails → Wi-Fi drops on a repeating pattern.
