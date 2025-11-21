# Troubleshooting Drill #6 – VPN Connects but No Internet or Internal Access  
**Difficulty:** Hard  

## Scenario  
VPN connects successfully, but all connectivity dies.

## Steps Taken  
1. Baseline internet works without VPN.  
2. Compared `ipconfig` before/after VPN.  
3. After VPN:  
   - DNS = 0.0.0.0  
   - Gateway missing  
   - Wi-Fi shows “Media disconnected”  
4. `ipconfig /flushdns` / `registerdns` → no change.  
5. Reset VPN adapter using:  
netsh int ip reset
netsh winsock reset
6. Reinstalled VPN client → routes + DNS fixed.

## Root Cause  
Corrupted VPN virtual adapter pushing invalid DNS and routes.

## Lesson Learned  
VPN issues are **usually routing/DNS problems**, not server issues.
