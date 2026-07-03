# Changelog — Audit Admin Toolkit

All notable changes to the Audit Admin Toolkit. Per-product versioning within suite **2026.1**.
Supersedes the legacy 6.x numbering.

## [1.0.4] — 2026-07-03

Suite **version-alignment** release. All six AuditToolkit suite products are advanced together
to a common **1.0.4** baseline so the suite ships as one coordinated release. No breaking
changes; upgrade in place via the native package.

### Changed
- **Unified suite versioning.** Audit Tool, CMDB API Data Collection, Asset Command Centre,
  Linux Security Lite, Switch Exposure Centre, and Audit Assurance Node are aligned at 1.0.4,
  replacing the previous mixed 1.0.2 / 1.0.3 lines.
- **Consistent licensing and legal.** The shared End User License Agreement, Liability
  Disclaimer and Indemnity, Copyright, and Licensing documents apply uniformly across the
  suite and are bundled with this product.
- **Documentation references official channels only.** Customer documentation references only
  the official AuditToolkit website and public release channel — no private source-control
  hosts. Internal cross-references were verified and the documents bundle is checked against
  its signed manifest.

### Carried forward (from 1.0.3)
- Documentation-accuracy pass: deployment described as native `.deb` / `.rpm` with PostgreSQL
  auto-provisioned; in-product Documents page ships in the packages.

### Packaging / artifacts
- **Server** as `deb` + `rpm`; **3 agents** (fleet, standalone, hypervisor) as Windows `msi`
  (embedded Python); customer-docs tarball; CycloneDX SBOM (`.cdx.json`);
  `SHA256SUMS-1.0.4.txt` and `SHA256SUMS-agents-1.0.4.txt`. Every artifact has a detached
  GPG signature (`.asc`).

### Known limitations
- **Windows MSIs are self-signed** — SmartScreen will show an untrusted-publisher warning.
  Packages install and run normally; public code-signing is a planned improvement.

## [1.0.3] — 2026-06-29
See the v1.0.3 release notes for the prior maintenance release (190 commits since 1.0.2;
console de-inlining refactor, documentation-accuracy pass, full server + agent package matrix).

## [1.0.2] and earlier
See the per-version release notes in the AuditToolkit-Docs release archive.
