# Security Hardening Audit — Project Instructions

## Purpose
This project contains PowerShell scripts to audit OS and application hardening settings on Windows hosts, aligned with the **ASD Essential Eight** maturity model — specifically the **hardening** controls (not patch management, MFA, admin restrictions, or backups).

## Scope
Hardening controls drawn from E8 guidance, including but not limited to:
- LSASS protection (PPL, Credential Guard)
- Hypervisor-Protected Code Integrity (HVCI / Memory Integrity)
- PowerShell logging (Script Block Logging, transcription)
- Process creation audit logging (command-line capture)
- Attack Surface Reduction (ASR) rules
- Windows Defender / Microsoft Defender for Endpoint settings
- SMB signing and hardening
- PowerShell execution policy and constrained language mode
- Remote Desktop and remote access hardening
- Windows Firewall state

## Target Environment
Scripts must run correctly on **both**:
- **Workstations**: Windows 10 / Windows 11
- **Servers**: Windows Server 2016 / 2019 / 2022

Where behaviour differs between workstation and server (e.g. minimum OS build thresholds), branch on `Win32_OperatingSystem.ProductType` (1 = Workstation, 3 = Server) and document the difference in the function comment.

## Script Behaviour

- Scripts are **audit-only** — they never change settings.
- All scripts require `#Requires -RunAsAdministrator`.

## Code Conventions

### Function structure
- One function per control check.
- Naming: `Get-<ControlName>Status`.
- Every function returns a `[PSCustomObject]` with at minimum:
  - `Check` — human-readable control name
  - `Enabled` — `$true` / `$false` (compliant or not)
  - `RawValue` — the raw registry or API value (aids debugging)
  - `Supported` — include when a version gate applies; `$false` means the check was skipped
- Use `-ErrorAction SilentlyContinue` on registry reads; a missing key means the feature is not configured (treat as non-compliant).

### Comments
- One comment block per function explaining **what it checks and why** — not line-by-line narration.
- Inline comments only for non-obvious logic (e.g. version gate thresholds, WMI quirks).

### Output
Output format is TBD. For now, pipe `$results` to `Format-List` at the end of each script. We will standardise this once a reporting approach is decided.

### Error handling
- Never silently discard unexpected errors — use `Write-Warning` for recoverable issues.
- Fatal errors (e.g. can't determine OS version) should `throw` with a clear message.

## Reference
- [ASD Essential Eight](https://www.cyber.gov.au/resources-business-and-government/essential-cyber-security/essential-eight)
- [ASD Essential Eight Maturity Model](https://www.cyber.gov.au/resources-business-and-government/essential-cyber-security/essential-eight/essential-eight-maturity-model)
