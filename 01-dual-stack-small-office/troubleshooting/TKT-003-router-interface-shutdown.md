# TKT-003: Internal server unreachable for all users - router interface shut down

| Field | Value |
|---|---|
| Priority | P2 (whole server LAN unreachable, multiple users impacted) |
| Category | Network - routing / interface |
| Reported by | Administration team |
| Affected service | All access to Server LAN 192.168.20.0/24 (IPv4 and IPv6) |
| Opened / resolved | 2026-10-20 / 2026-10-20 |
| Resolved at | Level 1 under change control |

## Symptom
Users report that the internal server cannot be reached by IPv4 or IPv6.

## Scope
- Every device on the Admin LAN is affected, so this is not a single-PC fault.
- Users can still reach their gateway (192.168.10.1), so the router is up.

## Investigation

| Test | Result | Conclusion |
|---|---|---|
| PC: `ping 192.168.10.1` | Success | Admin LAN and router G0/0 are fine |
| PC: `ping 192.168.20.10` | Failed | Problem is beyond the router |
| R1-HQ: `show ip interface brief` | G0/1 `administratively down down` | Interface disabled by configuration |
| R1-HQ: `show ip route` | No `C 192.168.20.0/24` route | Router no longer knows the server LAN |

Evidence: [before](../verification/TKT-003_R1-HQ_before.txt)

## Root cause
GigabitEthernet0/1 on R1-HQ had been shut down (`shutdown`). "Administratively down" means a configuration change, not a cable or hardware fault. When the interface went down, its connected route was removed from the routing table.

## Fix

    R1-HQ(config)# interface g0/1
    R1-HQ(config-if)# no shutdown

## Verification
- G0/1 `up up`, `C 192.168.20.0/24` back in the routing table: [after](../verification/TKT-003_R1-HQ_after.txt)
- PC-ADMIN ping to 192.168.20.10 successful

## Escalation decision
Resolved at Level 1 because the cause was clear and the fix was a single, reversible command. In production I would:
- Raise this as P2 and update the customer within the SLA response time
- Check the change log and with Level 2 before re-enabling, in case the shutdown was deliberate (planned maintenance)
- Escalate to Level 2 if the interface showed `down down` (physical fault), not `administratively down`

## Customer update sent
"We identified that a network port serving the server network had been disabled. It has been re-enabled and the server is reachable again. We are reviewing why the change happened so it does not recur."

## Prevention
- Restrict who can make config changes (AAA, Project 05)
- Send interface up/down events to Syslog and monitoring (SNMP) so outages raise an alert before users call (Project 04)

## CCNA objective
1.2 Interface and cable issues · 3.1 Interpret the components of the routing table
