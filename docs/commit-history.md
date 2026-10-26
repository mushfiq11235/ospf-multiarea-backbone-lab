# Suggested Commit History

A clean, readable history helps a reviewer follow how the project was built. These commits group the files logically. Run them from the repository root, in order.

## Message convention

```
<type>(<scope>): <short summary in imperative mood, max ~72 chars>

<optional body: what and why>
```

Types used: `chore`, `feat`, `docs`, `fix`.

## First-time setup

```bash
git init -b main
git config user.name "Your Name"
git config user.email "you@example.com"
```

## Initial commits (copy-paste)

```bash
git add .gitignore LICENSE
git commit -m "chore: add .gitignore and MIT license"

git add lab/OSPF_MultiAREA_RETRY.pkt
git commit -m "feat(lab): add Packet Tracer file for multi-area OSPF topology" \
           -m "Seven 1941 routers, four OSPF areas, two tunnels extending Area 0."

git add configs/raw configs/0*.cfg
git commit -m "feat(configs): add per-device running-configs for all seven routers" \
           -m "Includes the raw capture. Hostnames are kept exactly as captured."

git add verification/
git commit -m "docs(verification): add captured OSPF neighbor tables and command list" \
           -m "Neighbor output from the three ABRs showing FULL adjacencies, including tunnel neighbors."

git add docs/images/topology.png
git commit -m "docs(topology): add Packet Tracer topology image"

git add docs/native-virtual-link-reference.md
git commit -m "docs(backbone): add native virtual-link reference for comparison" \
           -m "Marked as untested. The lab itself uses GRE tunnels advertised in Area 0."

git add docs/portfolio-description.md docs/screenshots.md docs/commit-history.md
git commit -m "docs(portfolio): add portfolio text, screenshot checklist and commit guide"

git add README.md
git commit -m "docs(readme): add project overview, design, LSDB, verification and troubleshooting" \
           -m "Covers addressing plan, area design, OSPF architecture, backbone extension, challenges, lessons learned and future work."
```

## Connect to GitHub and push

Create an empty repository on GitHub first (no README, no license), then:

```bash
git remote add origin https://github.com/<your-username>/ospf-multiarea-backbone-lab.git
git push -u origin main
```

## Later commits (examples)

```bash
# After adding your own screenshots
git add docs/images/screenshots/
git commit -m "docs(verification): add neighbor, route and ping screenshots"

# After renaming hostnames in the .pkt
git add lab/ configs/
git commit -m "fix(lab): align router hostnames with topology diagram labels"

# After setting explicit router IDs
git commit -am "feat(configs): set explicit router-id on all routers"

# After testing the native virtual-link version
git add configs/native-vlink/
git commit -m "feat(configs): add native virtual-link variant of the lab" \
           -m "Compares OSPF_VL0 neighbors against the GRE tunnel version."

# After hardening
git commit -am "feat(security): add passive-interface on LANs and MD5 authentication"
```

## Tips

- One logical change per commit.
- Do not put "update" or "final" as the whole message.
- Tag a finished version: `git tag -a v1.0 -m "Initial published version" && git push --tags`
