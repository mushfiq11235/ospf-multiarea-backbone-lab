# Portfolio Description

Copy-paste text for GitHub, LinkedIn, a resume and interviews. All of it describes a **lab project**, not production work.

---

## GitHub "About" box (one line)

```
CCNA-level Cisco Packet Tracer lab: 4-area OSPF network with Area 0 extended across transit areas to reconnect isolated areas. Configs, LSDB notes, verification and troubleshooting included.
```

**Topics (tags):**

```
ospf  cisco  packet-tracer  ccna  networking  routing  network-engineering  lab  gre-tunnel  multi-area-ospf
```

---

## Recruiter-friendly description

**Multi-Area OSPF with Chained Backbone Extension (Cisco Packet Tracer)**

A hands-on networking lab I built while studying for the CCNA. The network has seven Cisco 1941 routers in four OSPF areas arranged in a chain, so only one area touches the backbone. I worked out why routes from the far areas were missing, extended Area 0 across two transit areas using tunnels between the ABRs, and confirmed that the far-area networks appeared as inter-area (`O IA`) routes. The repository includes device configs, an addressing plan, an explanation of the LSDB and LSA types involved, verification output, a troubleshooting guide, and a note on how my tunnel approach differs from a native OSPF virtual link.

**What it shows:** OSPF fundamentals, reading neighbor tables and routing tables, structured troubleshooting, and clear technical documentation.

---

## LinkedIn project blurb

> Built a multi-area OSPF lab in Cisco Packet Tracer as part of my CCNA preparation. The topology chains four areas, which leaves two of them disconnected from the backbone. I diagnosed the missing inter-area routes, restored Area 0 connectivity through two transit areas, and verified the result with neighbor adjacencies and `O IA` routes. Documented the design, LSDB behaviour, and troubleshooting steps on GitHub.
>
> Skills: OSPF · Cisco IOS · Routing · Network troubleshooting · Technical documentation

---

## Resume bullets (pick two or three)

- Designed and configured a 7-router, 4-area OSPFv2 lab in Cisco Packet Tracer, including three ABRs and a chained area layout.
- Diagnosed missing inter-area routes caused by areas lacking an Area 0 attachment, and restored backbone connectivity using two tunnels advertised into Area 0.
- Verified full OSPF adjacencies and inter-area (`O IA`) route propagation using `show ip ospf neighbor` and `show ip route`.
- Documented addressing plan, LSDB/LSA behaviour, and a troubleshooting playbook in a public GitHub repository.

---

## 30-second interview explanation

"It's a Packet Tracer lab I built for my CCNA practice. I chained four OSPF areas together, which breaks the rule that every area has to connect to Area 0. Areas 20 and 30 couldn't exchange routes because their ABRs had no backbone attachment. I fixed that by building tunnels between the ABRs and advertising the tunnel subnets in Area 0, so the backbone effectively reaches across the transit areas. Then I checked the neighbor tables and confirmed the Area 30 networks showed up as `O IA` routes."

---

## Questions to be ready for

**Is that a real OSPF virtual link?**
No, and I say so in the README. I used GRE tunnels with the tunnel subnets in Area 0. It achieves the same goal (an ABR with an active Area 0 adjacency), but a native virtual link uses `area X virtual-link <router-id>`, shows as `OSPF_VL0`, and needs no tunnel subnet. I documented the native version as an untested reference.

**Why do areas have to connect to Area 0?**
ABRs exchange inter-area routes through the backbone using Type 3 LSAs. That hub-and-spoke structure keeps inter-area routing loop-free.

**What happens if the transit area fails?**
The tunnel endpoints lose reachability, the tunnel goes down, and the backbone partitions. The far areas lose inter-area routes.

**Why is there no DR/BDR election?**
The links are point-to-point (serial and tunnel), so OSPF treats them as point-to-point networks. That is why the neighbor output shows `FULL/ -`.

**Why did the Area 30 routes show as `O IA`?**
They were learned from Type 3 Summary LSAs originated by an ABR, so they are inter-area routes rather than intra-area (`O`).

**Why are some router IDs not the highest interface IP?**
The router ID is chosen when the OSPF process starts and stays until the process is reset. I would set `router-id` explicitly next time.

**What would you do differently in production?**
Not chain the areas at all. Attach every area to Area 0, add authentication, passive interfaces, summarisation and basic device hardening.
