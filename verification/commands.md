# Verification Commands

Copy-paste lists for the Packet Tracer CLI. Run in privileged EXEC mode (`enable` first if prompted).

Some variants differ between Packet Tracer and real IOS. If a command is rejected, try a shorter form.

## Any router

```
show ip interface brief
show ip protocols
show ip ospf
show ip ospf neighbor
show ip ospf interface
show ip ospf database
show ip route
show ip route ospf
show running-config | section ospf
```

## Tunnel endpoints (ABR_Area0-Area10, ABR_10-20, ABR_a20-a30)

```
show interfaces tunnel 0
show interfaces tunnel 10
show interfaces tunnel 22
```

Use the tunnel number that exists on the router:

| Router | Tunnel interface(s) |
|---|---|
| ABR_Area0-Area10 | Tunnel0 |
| ABR_10-20 | Tunnel10, Tunnel22 |
| ABR_a20-a30 | Tunnel0 |

## Underlay reachability (tunnel destination must be reachable)

```
! On ABR_Area0-Area10
ping 192.168.13.2

! On ABR_10-20
ping 192.168.11.1
ping 192.168.23.2

! On ABR_a20-a30
ping 192.168.21.1
```

## Tunnel neighbor reachability

```
! On ABR_Area0-Area10
ping 172.16.10.2

! On ABR_10-20
ping 172.16.10.1
ping 172.18.10.2

! On ABR_a20-a30
ping 172.18.10.1
```

## End-to-end tests from PCs (Command Prompt tab)

```
ping 192.168.32.2        (from 192.168.0.2)
ping 192.168.0.2         (from 192.168.32.2)
ping 192.168.22.2        (from 192.168.10.2)
tracert 192.168.32.2     (from 192.168.0.2)
```

## What to look for

| Check | Expected |
|---|---|
| `show ip ospf neighbor` | All neighbors `FULL`, including tunnel neighbors on the ABRs |
| `show ip route` on ABR_10-20 | `O IA 192.168.30.0/24`, `.31.0/24`, `.32.0/24` via `Tunnel22` |
| `show ip ospf interface` on an ABR | Tunnel interfaces in Area 0, LAN/serial interfaces in their own areas |
| `show interfaces tunnel X` | `up/up` with the expected source and destination |

## Captured outputs in this repo

See [`captured-outputs/`](captured-outputs/).
