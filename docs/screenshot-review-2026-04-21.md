# Screenshot Review - 2026-04-21

## Source

- Source folder reviewed: `/home/dallas/SyncLab/AD LAB Screenshots/`
- Source files reviewed: 80 PNG screenshots
- Curated screenshots added to repo: 44 PNG screenshots

## Selection Approach

The source folder included a full troubleshooting trail with several repeated views of the same command output, dialog box, or ADUC screen. I kept the screenshots that best prove a distinct step or decision point and skipped near-duplicates when a clearer screenshot already covered the same evidence.

## Added Evidence Areas

### Windows 11 Client Join

Added to `screenshots/08-client-join/`:

- Windows 11 setup and OOBE
- Hyper-V lab switch attachment
- initial disconnected/APIPA client state
- static client IP and DNS configuration
- client DNS and ping validation
- computer rename to `WIN11-CLIENT1`
- domain join with restart requirement
- domain sign-in and post-join environment checks
- `Get-ADComputer` validation from `DC-1`
- ADUC computer-object visibility
- helpdesk user sign-in on the joined client
- RSAT optional-feature limitation
- helpdesk client DNS/connectivity/group-token validation

### Helpdesk Delegation Troubleshooting

Added to `screenshots/09-helpdesk-delegation/`:

- Helpdesk user present in ADUC
- Helpdesk user membership in `Domain Users` and `GG_Helpdesk`
- Lab Users OU state
- Delegation of Control Wizard flow for `GG_Helpdesk`
- `dsa.msc` runas/logon-type failure
- Local Security Policy and Group Policy logon-right review
- `Deny log on locally` check
- `gpupdate /force` success
- runas/UAC elevation boundary
- domain-controller internal connectivity and no-internet validation

## Not Added

Screenshots were not copied when they were:

- intermediate duplicates of the same command output
- earlier versions of a clearer later screenshot
- repeated UAC prompts without a new state
- unrelated to the final helpdesk/user/client validation path
- focused on temporary alternate test users or groups where the final `Helpdesk.User` / `GG_Helpdesk` screenshots provided better evidence

## Addressing Note

The repo now keeps the actual isolated lab subnet (`10.10.10.0/24`) in markdown and screenshots. This lab has no internet path, and the addresses shown are not sensitive.
