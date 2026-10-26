# Suggested Screenshots

Save images in `docs/images/screenshots/` using the file names below, then uncomment the matching lines in the README's Verification section (or add them yourself).

## Capture tips

- Use the **CLI** tab on each router and press Enter once so the prompt is visible in the image.
- Crop to the terminal output. Avoid capturing unrelated windows or personal information.
- Enlarge the Packet Tracer window first so text stays readable.
- Capture the output on the router named in the table; the hostname in the prompt is part of the evidence.

## Checklist

| # | File name | Device | Command / view | What it should show |
|---|---|---|---|---|
| 1 | `01-topology.png` | Workspace | Logical view, all areas visible | Areas, subnets, PCs, labels (a copy already exists as `docs/images/topology.png`) |
| 2 | `02-neighbors-abr-10-20.png` | ABR_10-20 | `show ip ospf neighbor` | Four `FULL` neighbors including `Tunnel10` and `Tunnel22` |
| 3 | `03-neighbors-abr-0-10.png` | ABR_Area0-Area10 | `show ip ospf neighbor` | `FULL` neighbors on `Serial0/0/0`, `Serial0/0/1`, `Tunnel0` |
| 4 | `04-neighbors-abr-20-30.png` | ABR_a20-a30 | `show ip ospf neighbor` | `FULL` neighbors on both serials and `Tunnel0` |
| 5 | `05-route-table-abr-10-20.png` | ABR_10-20 | `show ip route` | `O IA` routes for 192.168.30/31/32.0 via `Tunnel22` |
| 6 | `06-route-table-area0-r1.png` | Area0_R1 | `show ip route` | `O IA` routes for Areas 10, 20 and 30 |
| 7 | `07-route-table-router3.png` | Router3 | `show ip route` | `O IA` routes back toward Areas 0, 10, 20 |
| 8 | `08-ospf-database-abr-10-20.png` | ABR_10-20 | `show ip ospf database` | Separate Area 0, 10 and 20 databases, Router and Summary LSAs |
| 9 | `09-tunnel22-status.png` | ABR_10-20 | `show interfaces tunnel 22` | `up/up`, source `Serial0/0/0`, destination `192.168.23.2` |
| 10 | `10-ospf-interfaces-abr-10-20.png` | ABR_10-20 | `show ip ospf interface` | Interfaces in their correct areas, tunnels in Area 0 |
| 11 | `11-ping-end-to-end.png` | PC 192.168.0.2 | `ping 192.168.32.2` | Replies across all four areas (capture only if you ran it) |
| 12 | `12-before-fix.png` | ABR_a20-a30 or Area0_R1 | `show ip route` before Tunnel 2 | Area 30 routes absent (optional "before" evidence) |

## README embed lines

Uncomment these in the README once the files exist:

```markdown
![ABR_10-20 neighbors](docs/images/screenshots/02-neighbors-abr-10-20.png)
![O IA routes on ABR_10-20](docs/images/screenshots/05-route-table-abr-10-20.png)
![OSPF database on ABR_10-20](docs/images/screenshots/08-ospf-database-abr-10-20.png)
![End-to-end ping](docs/images/screenshots/11-ping-end-to-end.png)
```

## Priority order if you only take a few

1. `02` (neighbors on the middle ABR)
2. `05` (the `O IA` routes)
3. `08` (LSDB)
4. `11` (end-to-end ping)
