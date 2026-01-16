# Active Directory Lab (Windows Server 2022 + Hyper-V)

Hands-on Active Directory home lab built on Windows Server 2022 in Hyper-V.  
Documented with repeatable build steps, validation checks, troubleshooting notes, and sanitized screenshots for portfolio use.

## Quick start (recommended order)

1. **Lab 01 — Build Domain Controller (DC-1)**
2. **Lab 02 — Join a Windows client to the domain**
3. **Lab 03 — Basic AD operations (OUs, users, groups, basic GPO)**

> Tip: Each lab includes **Validation** so you can prove it’s working (commands + expected results).

## What this repo demonstrates

- Windows Server 2022 Domain Controller build (AD DS + DNS)
- Lab networking fundamentals (internal subnet, static IP, DNS pointing to the DC)
- Domain join workflow for a client machine
- Core AD operations: OUs, users, groups, and basic Group Policy
- Documentation quality aligned with IT support / junior sysadmin expectations

## Repository layout

- `docs/` — documentation standards
  - `screenshot-rules.md`
  - `screenshot-naming.md`
- `screenshots/` — evidence organized by phase
  - `01-vm-create/`
  - `02-install/`
  - `03-network/`
  - `04-ad-ds/`
  - `05-domain-join/`
  - `06-users-groups/`
  - `07-gpos/`
- `labs/` — step-by-step lab writeups
  - `01-build-domain-controller.md`
  - `02-join-client.md`
  - `03-basic-ad-ops.md`

## Screenshot safety (sanitization)

Allowed:
- VM settings and lab configuration screens
- Internal IPs (e.g., `10.10.10.x`)
- Lab domain names (e.g., `lab.local`)

Do not publish:
- Public IP addresses, Wi-Fi SSIDs, personal email addresses
- License keys, serial numbers, hardware IDs

If sensitive information appears:
- Re-take the screenshot with the sensitive area hidden, or blur it before uploading.

## Screenshot naming convention

Format: `<section>-<step>_<short-description>.png`

Examples:
- `01-01_hyperv-new-vm.png`
- `03-04_ipconfig-dc1.png`

See `docs/screenshot-naming.md` for the full convention.

