# CMDB API Data Collection Tool - v1.0.1 Release Documentation

**Release Version:** 1.0.1  
**Release Date:** May 15, 2026  
**Status:** General Availability (Patch Release)

---

## Overview

This patch release of the CMDB API Data Collection Tool focuses on validation correctness,
security hardening, and release-governance improvements.

v1.0.1 is intended as an in-place upgrade for customers currently on v1.0.0.

---

## What's Included

### Inline Release Documentation

- `RELEASE-v1.0.1.md` (this document)
- `CHANGELOG.md`
- `36-release-deployable-security-gate.md`
- `LICENSE-EULA.md`
- `README.md`

### Platform Release Assets

- Core server packages (Linux, Windows)
- Managed agent packages (Windows, Linux, macOS, BSD)
- Targeted in-place update bundles

All downloadable binaries and installers for this release are published in the customer
release portal folder:

- [Customer release assets folder](https://audittoolkitlabs.sharepoint.com/:f:/s/AuditToolkitCustomerReleasePortal/IgBbPUcfpK_qQJxU0n01GunBAQpAGOt2v7am73tUoYlxw5E?e=USfV4u)

Repository-hosted release access:

- [Release folder in this repository](https://github.com/AuditToolkitLabs/releases/tree/main/downloads/CMDB%20API%20Data%20Collection%20Tool/v1.0.1)
- [Published downloads page](https://audittoolkitlabs.github.io/releases/v1.0.1/)

---

## What's New in v1.0.1

### Fixed

- Webhook URL validation now handles test-mode DNS failures safely
- Test-mode DNS bypass is limited to RFC 2606 example domains only
- URL validation checks were tightened to reject invalid URI patterns

### Security and Release Assurance

- Added release-scope security gate in CI/CD
- Improved CodeQL configuration reliability and scan consistency
- Added dual-scope reporting (deployable scope and full-workspace transparency)
- Confirmed zero deployable-scope release blockers (CodeQL and Bandit)

Refer to `CHANGELOG.md` for the full detail list.

---

## Upgrade Guidance

### Recommended Upgrade Path

Upgrade from v1.0.0 to v1.0.1 using the targeted update bundle for your platform.

Example Linux update:

```bash
python3 apply_targeted_update.py \
  --bundle cmdb-tool-update-linux-1.0.1.tar.gz \
  --install-root /opt/cmdb-tool \
  --config-path /etc/cmdb-tool/cmdb-tool.conf
```

Example Windows update:

```powershell
python .\apply_targeted_update.py `
  --bundle .\cmdb-tool-update-windows-1.0.1.zip `
  --install-root "C:\Program Files\CmdbTool" `
  --config-path "C:\ProgramData\CmdbTool\cmdb-tool.env"
```

Use `--dry-run` first if you want to preview backup and patch actions.

---

## Support

For release changes and patch details, see `CHANGELOG.md`.

For security-gate evidence and OWASP release posture, see
`36-release-deployable-security-gate.md`.

For licence and permitted-use obligations, see `LICENSE-EULA.md`.

---

**Last Updated:** May 15, 2026  
**Prepared for:** Release Repository Distribution
