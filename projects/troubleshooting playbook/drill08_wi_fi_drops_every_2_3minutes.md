---
layout: page
title: "Troubleshooting Drill #8 – Wi-Fi Drops Every 2-3 Minutes (Single User)"
permalink: /projects/troubleshooting%20playbook/drill08/
---

1. Confirm the Drop Pattern

Before we treat the patient, make sure they’re actually sick.

🧪 Continuous Ping
ping -t <gateway_or_domain_controller_IP>


What you’re looking for:

Normal responses ➜ then sudden Request Timed Out

Pattern of drops every 2–3 minutes
That’s your neon sign for:

🚨 DHCP lease ≈ 180 seconds

🚨 802.1X reauthentication cycle

🚨 Wi-Fi adapter power saving toggling on/off

2. Inspect the Client’s IP + Lease

Run:

ipconfig /all


On the Wireless adapter, verify:

IPv4

Gateway

DHCP enabled

DNS

Lease Obtained / Lease Expires

MAC address (just in case the NIC is spoofing)

🔑 Huge clue from your drill:

Lease duration was literally 3 minutes → matching the drop interval.

If you see leases measured in:

Seconds

1–3 minutes

Something absurd like "50 seconds"

→ Someone borked the DHCP scope.

3. Check the DHCP Scope (Server Side)

Hop on the DHCP server and examine the scope for that subnet:

Look for:

Lease duration (should be hours, not minutes)

Scope nearly full (90%+ used can cause weird behavior)

Any “intern-fingerprints” like:

Lease = 180 seconds

DNS/WINS misconfig

Rogue reservations

Fix:

Set lease back to 8 hours (common default)

Apply changes

💡 Good to know:
Changing the lease does not instantly drop clients — they learn the new duration at next renewal.

If only one laptop is having issues even after fixing DHCP → you know the problem follows the client.

4. Compare Wired vs Wireless

Have the user plug into Ethernet:

If wired is:

Stable

Pings clean

Normal DHCP lease

No auth issues

Then you’ve just proven the issue is:

➜ Wi-Fi NIC
➜ Wireless auth (802.1X)
➜ Driver
➜ Power management

This is where the real fun starts.

5. Event Viewer: The Real Truth Serum

Open:

Event Viewer → Windows Logs → System


Filter by sources:

DHCP-Client

WLAN-AutoConfig

EAPOL

Microsoft-Windows-8021X

Netwtwxx (Intel wireless driver errors)

Look for:
DHCP-Client:

1001 – renewal failed

1002 – DHCPNACK

WLAN-AutoConfig:

“Disconnected due to authentication failure”

“Roamed to new BSSID” (driver confusion)

“Reason: 0x00038002” (EAP timeout)

802.1X:

Reauthentication failure

Bad certificate

TLS handshake timed out

🧠 Pro insight:
If auth fails → NIC drops → DHCP tries again → everything looks broken in a loop.

6. Fix the Wi-Fi NIC (The Usual Villain)
6.1 Update the wireless driver

Device Manager → Network Adapters → Wi-Fi card → Properties

Check:

Version

Date

Compare with known good version

Update to approved version

🔥 Wireless drivers are notorious for:

802.1X drops

Weird lease failures

Periodic reauth crashes

6.2 Disable Power Saving (Always do this)

Wireless NIC → Properties → Power Management:

❌ Uncheck “Allow the computer to turn off this device to save power”

6.3 Validate 802.1X / Certs (If using WPA2-Enterprise)

Check:

Machine cert installed

Cert not expired

Trust chain valid

Correct authentication method

Time is correct

If the cert expired yesterday → say hello to 3-minute drop hell.

7. Retest (The Victory Lap)

Disconnect wired → connect Wi-Fi → run:

ping -t <gateway_or_DC>


Watch for:

Clean, stable responses

No pattern-based timeouts

Event Viewer logs staying quiet

Let it run 5–10 minutes.

If clean → you fixed it.
If still dropping → go deeper (NIC replacement or network policy issue).

✅ Root Cause from the Drill

Outdated wireless driver → 802.1X reauthentication failures → DHCP renewal fails → Wi-Fi drops every 2–3 minutes.

Classic. Predictable. Deadly.
It’s basically the “boss fight” of Wi-Fi troubleshooting.
