# Active Directory Lab — Windows Server 2022 (Hyper-V)

Step-by-step Active Directory home lab on Windows Server 2022 (Hyper-V), documented with repeatable build notes, screenshots, and troubleshooting.

## Repo Structure

- docs/
  - screenshot-rules.md
  - screenshot-naming.md
- screenshots/
  - 01-vm-create/
  - 02-install/
  - 03-network/
  - 04-ad-ds/
  - 05-domain-join/
  - 06-users-groups/
  - 07-gpos/
- labs/
  - 01-build-domain-controller.md
  - 02-join-client.md
  - 03-basic-ad-ops.md

## Screenshot Safety Rules (Summary)

OK to show:
- VM settings
- Internal IPs like 10.10.10.x
- Lab domain name like lab.local

Avoid:
- Public IPs, Wi-Fi SSID
- Personal emails
- License keys, serial numbers

If sensitive info appears:
- Blur it (Paint) or re-take with the area hidden.

## Screenshot Naming

Examples:
- 01-01_hyperv-new-vm.png
- 03-04_ipconfig-dc1.png

See `docs/screenshot-naming.md` for full convention.
