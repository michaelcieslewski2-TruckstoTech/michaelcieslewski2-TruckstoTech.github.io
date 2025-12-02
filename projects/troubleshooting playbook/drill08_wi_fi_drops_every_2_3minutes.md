---
layout: page
title: "Troubleshooting Drill #8 – Wi-Fi Drops Every 2-3 Minutes (Single User)"
permalink: /projects/troubleshooting%20playbook/drill08/
---

🛜 Wi-Fi Drops Every 2–3 Minutes (Single User) — Troubleshooting Playbook
🧩 Symptom Pattern

User is on Wi-Fi only (wired works fine or isn’t used)

Connection works for 1–3 minutes, then:

- Drops

- Times out

- Hangs

- Reconnects automatically

Other users on the same Wi-Fi do not have the issue

1. Confirm the Pattern
🔄 Continuous Ping Test
ping -t <gateway_or_domain_controller_IP>


  Watch for:

   - Repeating Request timed out every 2–3 minutes

   - If it’s consistent → think:

   - DHCP lease too short

   - 802.1X periodic reauthentication

   - Wi-Fi adapter power-saving toggling

2. Check the IP & DHCP Lease

      Run:

      ipconfig /all


      For the Wireless adapter, verify:

           - IPv4 address

   - Subnet mask

   - Default gateway

   - DNS servers

   - DHCP Enabled (Yes/No)

   - Lease Obtained / Lease Expires

  🔑 Clue from drill:

  Lease duration was only 3 minutes, matching the drop interval.

  A lease this short usually means:

   - Misconfigured DHCP scope

   - DHCP scope nearly exhausted

   - Rogue settings pushed by GPO or a profile

3. Verify the DHCP Scope (Server-Side)

  Check:

   - Lease duration (should be hours, not minutes)

   - Scope utilization (avoid 90%+)

   - Recent configuration changes

   - Incorrect policies or reservations

  Fix:

  Set lease duration back to something normal (e.g., 8 hours)

  Apply changes

  Clients pick up new leases automatically next renewal

  If only this device still drops → the issue is client-side.

4. Compare Wired vs Wireless
  Plug in Ethernet and test:

   - Stable ping on wired?

   - Normal DHCP lease?

   - No disconnects?

  If wired works fine → the problem is Wi-Fi specific:

   - Wireless NIC

   - 802.1X authentication

   - Driver

   - Power settings

5. Event Viewer: Client-Side Truth

  Path:

  Event Viewer → Windows Logs → System


  Filter sources:

   - DHCP-Client

   - WLAN-AutoConfig

   - EAPOL

   - Microsoft-Windows-8021X

   - NetwtwXX (Intel driver logs)

  Look for:
   - DHCP-Client

   - Event ID 1001 — renewal failure

   - Event ID 1002 — DHCPNACK

   - Wireless

   - Disconnected: authentication failure

   - Roaming/reconnect loops

   - 802.1X / EAP

   - Reauthentication timed out

   - TLS negotiation failed

   - Certificate not found/expired

  If failures always happen at the 2–3 minute mark, you’re looking at:

   - Bad reauthentication attempts

   - Driver crash

   - Cert or supplicant issue

6. Fix the Wi-Fi Side
  6.1 Update the Wireless Driver

   - Device Manager → Network Adapters

   - Compare version/date to approved version

   - Update driver

   - Reboot

   - Outdated Wi-Fi drivers notoriously cause:

   - 802.1X reauth loops

   - DHCP lease drops

   - Random disconnects

  6.2 Disable Power Saving on the NIC

   - Device Manager → Adapter → Properties → Power Management

  Uncheck:

   - Allow the computer to turn off this device to save power

   - This setting can cause periodic NIC resets.

  6.3 Validate 802.1X / Certificates

  (If using WPA2-Enterprise)

  Check:

   - Machine/user cert exists

   - Cert is not expired

   - Trusted root CA installed

   - Correct EAP/PEAP settings

   - System time is accurate

   - Even a slightly expired cert will cause constant reauth failures.

7. Retest

  Disconnect wired → reconnect to Wi-Fi → run:

  ping -t <gateway_or_dc_IP>


  Let it run 5–10 minutes.

  Confirm:

  No repeating drops

  Stable latency

  Event Viewer stays clean

✅ Root Cause from the Drill

Outdated wireless driver → 802.1X reauthentication failures → DHCP renewal fails → Wi-Fi disconnects every 2–3 minute
It’s basically the “boss fight” of Wi-Fi troubleshooting.
 
 


