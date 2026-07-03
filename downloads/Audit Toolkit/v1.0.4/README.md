# Audit Admin Toolkit 1.0.4

Suite release **2026.1** · Product version **1.0.4** · Channel **GA**

The Audit Admin Toolkit is a security audit & compliance platform: a central **server**
(web console, reporting, scheduling, CVE/NVD) plus deployable **agents** (fleet, standalone,
hypervisor).

## About this release

1.0.4 is a **suite version-alignment** release. All six AuditToolkit suite products —
Audit Admin Toolkit, CMDB API Data Collection, Asset Command Centre, Linux Security Lite,
Switch Exposure Centre, and Audit Assurance Node — are advanced together to a common **1.0.4**
baseline so the suite ships as one coordinated release. **No breaking changes**; upgrade in
place via the native package.

## What's in this release

| Component | Linux (deb / rpm) | Windows (msi) |
|---|---|---|
| **Server** (main tool) | `audit-tool_1.0.4_amd64.deb` · `audit-tool-1.0.4-1.x86_64.rpm` | — |
| **Fleet agent** | — | `audit-fleet-agent-1.0.4-windows-x64.msi` |
| **Standalone agent** | — | `audit-standalone-agent-1.0.4-windows-x64.msi` |
| **Hypervisor agent** | — | `audittoolkit-hypervisor-agent-1.0.4-windows-x64.msi` |

Also included: `audit-tool-customer-docs-1.0.4.tar.gz` (customer documentation set),
`audit-tool-1.0.4.cdx.json` (CycloneDX SBOM), and the `SHA256SUMS-1.0.4.txt` /
`SHA256SUMS-agents-1.0.4.txt` checksum manifests. Every artifact ships with a detached
GPG signature (`.asc`) on the release.

The server/agent packages are **self-contained** — they bundle a pinned Python 3.12 runtime /
offline wheelhouse, so no system Python is required.

## Install

### Server
```bash
# Debian / Ubuntu
sudo apt install ./audit-tool_1.0.4_amd64.deb
# RHEL / Rocky / AlmaLinux / Fedora
sudo dnf install ./audit-tool-1.0.4-1.x86_64.rpm
```
The package provisions PostgreSQL, installs the systemd unit, bundles the customer-docs set
(in-product Documents page), and runs database migrations on upgrade.

### Agents — Windows
```powershell
msiexec /i audit-fleet-agent-1.0.4-windows-x64.msi
# Hypervisor agent enrolls at install time:
msiexec /i audittoolkit-hypervisor-agent-1.0.4-windows-x64.msi SERVERURL=https://host:8095 TOKEN=<enrollment>
```

## Verify checksums
```bash
sha256sum -c SHA256SUMS-1.0.4.txt
sha256sum -c SHA256SUMS-agents-1.0.4.txt
```

## Upgrade
Upgrade in place via the native package (deb/rpm); migrations run automatically.
**No breaking changes** from 1.0.3.

## Signing & status
- **GA release** — binaries install and run normally.
- **Windows MSIs** — currently **self-signed** (built with a self-signed AuditToolkit cert,
  not a publicly-trusted CA). Windows/SmartScreen shows an untrusted-publisher warning on
  install; the packages are otherwise fully functional. Public code-signing is a planned
  improvement.
- **Linux deb/rpm** and all artifacts ship with **detached GPG signatures** (`.asc`) and
  are covered by the signed `SHA256SUMS` manifests.

## Documentation
The curated customer documentation set is bundled in the packages and served by the in-product
**Documents** page — including the Deployment Pack, Operations Runbook, and Troubleshooting
Guide. The customer-docs tarball is also attached to this release.

See **CHANGELOG.md** for the list of changes in this release.
