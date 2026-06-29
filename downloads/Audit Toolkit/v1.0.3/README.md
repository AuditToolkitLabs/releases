# Audit Tool 1.0.3

Suite release **2026.1** · Product version **1.0.3** · **Pre-release** (see _Signing & status_)

The Audit Tool is a security audit & compliance platform: a central **server** (web console,
reporting, scheduling, CVE/NVD) plus deployable **agents** (fleet, standalone, hypervisor).

## What's in this release

| Component | Linux (deb / rpm) | Windows (msi) |
|---|---|---|
| **Server** (main tool) | `audit-tool_1.0.3_amd64.deb` · `audit-tool-1.0.3-1.x86_64.rpm` | — |
| **Fleet agent** | `fleet-agent_1.0.3_amd64.deb` · `fleet-agent-1.0.3-1.x86_64.rpm` | `audit-fleet-agent-1.0.3-windows-x64.msi` |
| **Standalone agent** | `standalone-agent_1.0.3_amd64.deb` · `standalone-agent-1.0.3-1.x86_64.rpm` | `audit-standalone-agent-1.0.3-windows-x64.msi` |
| **Hypervisor agent** | `hypervisor-agent_1.0.3_amd64.deb` · `hypervisor-agent-1.0.3-1.x86_64.rpm` | `audittoolkit-hypervisor-agent-1.0.3-windows-x64.msi` |
| **In-place update bundles** | `audit-toolkit-1.0.3-application.tar.gz` · `audit-toolkit-1.0.3-audits.tar.gz` | |

All server/agent packages are **self-contained** — they bundle a pinned Python 3.12 runtime /
offline wheelhouse, so no system Python is required.

## Install

### Server
```bash
# Debian / Ubuntu
sudo apt install ./audit-tool_1.0.3_amd64.deb
# RHEL / Rocky / AlmaLinux / Fedora
sudo dnf install ./audit-tool-1.0.3-1.x86_64.rpm
```
The package provisions PostgreSQL, installs the systemd unit, bundles the customer-docs set
(in-product Documents page), and runs database migrations on upgrade.

### Agents — Linux
```bash
sudo apt install ./fleet-agent_1.0.3_amd64.deb        # or: dnf install ./fleet-agent-1.0.3-1.x86_64.rpm
```

### Agents — Windows
```powershell
msiexec /i audit-fleet-agent-1.0.3-windows-x64.msi
# Hypervisor agent enrolls at install time:
msiexec /i audittoolkit-hypervisor-agent-1.0.3-windows-x64.msi SERVERURL=https://host:8095 TOKEN=<enrollment>
```

## Verify checksums
```bash
sha256sum -c SHA256SUMS-1.0.3.txt
```

## Upgrade
Upgrade in place via the native package (deb/rpm); migrations run automatically.
**No breaking changes** from 1.0.2.

## Signing & status (why this is a pre-release)
- **Windows MSIs** — internal **test-signed** (self-signed AuditToolkit cert; SHA-256, DigiCert-timestamped). **Not publicly trusted** — SmartScreen will warn. Intended for internal/beta/lab use.
- **Linux deb/rpm/tar** — **unsigned** this round (GPG / public signing to follow).
- **Release-gate** (install + UI/login on VM targets) — not yet run.
- **Hypervisor agent** — built at 1.0.3, but its internal `version.py` reads `0.1.0` (separate product; to be reconciled).
- The oversized `full.tar.gz` update bundle is intentionally omitted; the **deb/rpm are the canonical full installs**.

## Documentation
The curated customer documentation set is bundled in the packages and served by the in-product
**Documents** page — including the Deployment Pack, Operations Runbook, and Troubleshooting Guide.

See **CHANGELOG.md** for the list of changes in this release.
