# Native OSPF Virtual Link Reference (Untested)

> **Status: reference only. This was not built or tested in the Packet Tracer lab.** The lab itself uses GRE tunnels advertised in Area 0. This page shows what the native-command version would look like so the two approaches can be compared. Verify it in Packet Tracer before relying on it.

## Why this page exists

A native OSPF virtual link is configured with `area <transit-area> virtual-link <remote-router-id>` on both ABRs. It does not use a tunnel interface or an extra subnet. Because it identifies the peer by **router ID**, the IDs must be known and stable.

## Router IDs observed in the lab

| Router | Router ID |
|---|---|
| ABR_Area0-Area10 | 192.168.10.1 |
| ABR_10-20 | 192.168.21.1 |
| ABR_a20-a30 | 192.168.31.1 |

Set these explicitly (`router-id`) before building virtual links, then run `clear ip ospf process`.

## Virtual link 1: Area 0 ↔ ABR_10-20 through Area 10

```
! ABR_Area0-Area10
router ospf 1
 router-id 192.168.10.1
 area 10 virtual-link 192.168.21.1

! ABR_10-20
router ospf 1
 router-id 192.168.21.1
 area 10 virtual-link 192.168.10.1
```

## Virtual link 2: ABR_10-20 ↔ ABR_a20-a30 through Area 20

```
! ABR_10-20
router ospf 1
 area 20 virtual-link 192.168.31.1

! ABR_a20-a30
router ospf 1
 router-id 192.168.31.1
 area 20 virtual-link 192.168.21.1
```

**Open question to test:** the second link starts on ABR_10-20, which itself reaches Area 0 only through the first link. Check in the simulator that the chained arrangement comes up before treating it as valid.

## Verification commands

```
show ip ospf virtual-links
show ip ospf neighbor
show ip route ospf
```

Expected differences from the tunnel version:

- `show ip ospf virtual-links` reports the link state (it shows nothing for a GRE tunnel).
- The neighbor interface appears as `OSPF_VL0` instead of `TunnelX`.
- No `interface Tunnel` configuration or tunnel subnet is needed.

## Requirements and restrictions

- Both endpoints must be ABRs.
- The transit area must be a regular area (not stub or NSSA).
- The transit area must have a working route between the two router IDs.

## Tunnel version versus native version

| | GRE tunnel in Area 0 (lab) | Native virtual link |
|---|---|---|
| Config | Tunnel interface + `network ... area 0` | `area X virtual-link <RID>` |
| Neighbor interface | `TunnelX` | `OSPF_VL0` |
| Extra subnet | Yes | No |
| Encapsulation overhead | GRE | None |
| Peer identified by | Tunnel source/destination IPs | Router ID |
| Typical real-world view | Workaround | Temporary fix; redesign preferred |
