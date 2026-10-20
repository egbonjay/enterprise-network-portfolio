# TKT-001: Interface command rejected - wrong IOS mode
Severity: P4 | Project: 01 | Date: 2026-10-20 | Time to resolve: 2 min

## Symptom
`interface g0/1` returned `% Invalid input detected at '^' marker.`
G0/1 showed `unassigned` and `administratively down` in `show ip interface brief`.

## Scope
Only R1-HQ G0/1 (server LAN). G0/0 (admin LAN) was already up/up.

## Investigation
The prompt was `R1-HQ#` (privileged EXEC mode). Interface commands are only accepted in global configuration mode (`R1-HQ(config)#`).

    R1-HQ#int g0/1
              ^
    % Invalid input detected at '^' marker.

## Root cause
The command was entered in the wrong IOS mode, so G0/1 was never configured.

## Fix

    R1-HQ# configure terminal
    R1-HQ(config)# interface g0/1
    R1-HQ(config-if)# description LAN-SERVERS to SRV-INTERNAL
    R1-HQ(config-if)# ip address 192.168.20.1 255.255.255.0
    R1-HQ(config-if)# ipv6 address 2001:db8:10:20::1/64
    R1-HQ(config-if)# ipv6 address fe80::1 link-local
    R1-HQ(config-if)# no shutdown

## Verification

    R1-HQ#show ip interface brief
    GigabitEthernet0/0     192.168.10.1    YES manual up    up
    GigabitEthernet0/1     192.168.20.1    YES manual up    up

Full output: [R1-HQ_show-ip-int-brief.txt](../verification/R1-HQ_show-ip-int-brief.txt)

## Prevention
Check the prompt before entering commands. Re-run `show ip interface brief` after every interface change.

## CCNA objective
1.1 Explain the role and function of network components (IOS CLI modes)
