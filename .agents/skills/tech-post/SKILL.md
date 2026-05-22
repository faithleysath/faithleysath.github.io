---
name: tech-post
description: Use when creating or editing technical blog posts, troubleshooting notes, homelab plans, network/NAT/router/firewall/DNS content, Linux/tool configuration, research notes, or experiment records.
---

# Tech Post

Use for technical practice, troubleshooting, homelab, NAS, networking, NAT, routers, tunnels, firewalls, DNS, Linux/tool configuration, research notes, and experiment records.

## Paths

- Homelab/NAS/self-hosting: `content/tech/homelab/`
- Network/NAT/router/tunnel/firewall/DNS: `content/tech/network/`
- System tools/app configuration/troubleshooting: `content/tech/tools/`
- Research/reproduction/experiments: `content/tech/research/`
- Raw logs/private diagnostics/backups: `content/_archive/`
- Template: `content/_archive/templates/tech-post.md`

## Structure

Use only the sections that fit the post:

- problem background
- environment
- solution
- steps
- pitfalls
- final conclusion
- references

## Rules

- Keep large raw logs, private diagnostics, secrets, and temporary command output in `_archive`; public pages should include only necessary excerpts and summaries.
- If editing `content/tech/network/nftables-nat/index.html`, preserve the UTF-8 document head because it is a standalone HTML page.
- Update section indexes when adding a notable post.
- Validate with `npx --yes @faithleysath/blogx build --profile`.
