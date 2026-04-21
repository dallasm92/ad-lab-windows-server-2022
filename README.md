# Active Directory Lab (Windows Server 2022 + Hyper-V)

Hands-on Active Directory home lab built on Windows Server 2022 in Hyper-V.  
Documented with repeatable build steps, validation checks, troubleshooting notes, and sanitized screenshots for portfolio use.
Last reviewed: April 21, 2026

## Quick start (recommended order)

1. **Lab 01 — Build Domain Controller (DC-1)**
2. **Lab 02 — Join a Windows client to the domain**
3. **Lab 03 — Basic AD operations (OUs, users, groups, basic GPO)**
4. **Lab 04 — Helpdesk delegation troubleshooting**

> Tip: Each lab includes **Validation** so you can prove it’s working (commands + expected results).

## What this repo demonstrates

- Windows Server 2022 Domain Controller build (AD DS + DNS)
- Lab networking fundamentals (internal subnet, static IP, DNS pointing to the DC)
- Domain join workflow for a client machine
- Core AD operations: OUs, users, groups, and basic Group Policy
- Helpdesk user/group validation from a domain-joined Windows 11 client
- Troubleshooting around username format, missing RSAT tools, domain-controller logon rights, Group Policy refresh, and UAC/elevation boundaries
- Documentation quality aligned with IT support / junior sysadmin expectations

## Hiring Manager Quick View

| Review area | Evidence |
|---|---|
| Server build | Hyper-V VM creation, Windows Server install, static IP, AD DS/DNS promotion |
| Client support | Windows 11 client setup, static IP/DNS, domain join, domain sign-in validation |
| Identity basics | Helpdesk user, `GG_Helpdesk` group, ADUC membership validation |
| Troubleshooting | wrong username format, no internet/RSAT limitation, logon-right failure, `gpupdate /force`, UAC/elevation boundary |
| Proof style | 100+ screenshots organized by lab phase with step-by-step markdown labs |

## Repository layout

- `docs/` — documentation standards
  - `screenshot-rules.md`
  - `screenshot-naming.md`
  - `screenshot-review-2026-04-21.md`
- `screenshots/` — evidence organized by phase
  - `01-vm-create/`
  - `02-install/`
  - `03-network/`
  - `04-ad-ds/`
  - `05-domain-join/`
  - `06-users-groups/`
  - `07-gpos/`
  - `08-client-join/`
  - `09-helpdesk-delegation/`
- `labs/` — step-by-step lab writeups
  - `01-build-domain-controller.md`
  - `02-join-client.md`
  - `03-basic-ad-ops.md`
  - `04-helpdesk-delegation-troubleshooting.md`

## Screenshot safety (sanitization)

Allowed:
- VM settings and lab configuration screens
- Isolated lab IPs (e.g., `10.10.10.x`)
- Lab domain names (e.g., `lab.local`)

Do not publish:
- Public IP addresses, Wi-Fi SSIDs, personal email addresses
- License keys, serial numbers, hardware IDs

If sensitive information appears:
- Re-take the screenshot with the sensitive area hidden, or blur it before uploading.

Current evidence note:
- AD domain-controller and OU/group screenshots are present and cleaned for portfolio use.
- Client-domain-join and helpdesk delegation troubleshooting evidence are now present.
- The April 21 screenshot intake reviewed 80 source screenshots and added 44 curated screenshots.
- Current GPO evidence focuses on domain-controller logon-right troubleshooting. Workstation baseline policy enforcement is intentionally left as a future lab topic rather than claimed here.
- The repo documents the actual isolated lab subnet because the lab has no internet path and the addressing is not sensitive.

## Screenshot naming convention

Format: `<section>-<step>_<short-description>.png`

Examples:
- `01-01_hyperv-new-vm.png`
- `03-04_ipconfig-dc1.png`

See `docs/screenshot-naming.md` for the full convention.
