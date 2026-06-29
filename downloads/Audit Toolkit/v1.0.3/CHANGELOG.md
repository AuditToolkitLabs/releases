# Changelog — Audit Tool

All notable changes to the Audit Tool. Per-product versioning within suite **2026.1**;
the Audit Tool leads the suite uplift. Supersedes the legacy 6.x numbering.

## [1.0.3] — 2026-06-29

Maintenance release within suite 2026.1 (**190 commits since 1.0.2**).
No breaking changes; upgrade in place via the native `.deb` / `.rpm`.

### Added
- Web **security hardening** and admin capabilities (`feat(web/security)`, `feat(admin)`, `feat(web/admin)`).
- Console / UI enhancements (`feat(web/ui)`, `feat(ui)`).
- GUI Trust Pack updates (`feat(gui-trust)`).
- In-product help and document-to-document navigation in the Documents reader.

### Fixed
- **Asset Discovery** — the dashboard "Open Scheduled Scans" shortcut now opens the Scheduled
  Scans view (previously targeted the wrong panel).
- **Console / admin / terminal / remediation** — numerous page, panel, and workflow fixes,
  including restoration of form controls stripped during the monolith→per-page split
  (deploy mode/type, backfill, history/schedule filters, logs search).
- **Packaging** — ship the curated customer-docs set inside the `.deb`/`.rpm` (verified against
  its signed manifest) so the in-product Documents page works on a real install; pin the RPM to
  `python3.12` to match the offline wheelhouse.

### Changed
- **Console de-inlining refactor** — inline `style=`/`on*=` extracted to external CSS/JS across
  `web/templates` and `web/static` (no-inline-frontend standard; ~30 refactor commits).
- **Documentation accuracy pass** — customer docs corrected against shipping code: native
  **`.deb` (primary) / `.rpm` (secondary)** with PostgreSQL auto-provisioned (Docker/Kubernetes
  are dev/test only; no server MSI or OVA appliance); qualitative capability metrics; OWASP
  scorecard and dependency versions aligned to the current build.

### Packaging / artifacts
- Full matrix published: **server + 3 agents** as `deb` + `rpm`; **3 agents** as Windows `msi`
  (embedded Python); in-place `application` / `audits` update tarballs; `SHA256SUMS-1.0.3.txt`.

### Known limitations (pre-release)
- Signing deferred — MSIs are internal **test-signed**; Linux packages **unsigned**.
- Release-gate (install + UI/login on VM targets) not yet run.
- Hypervisor agent `version.py` reads `0.1.0` vs the `1.0.3` package/MSI stamp.

## [1.0.2] and earlier
See the per-version release notes in the AuditToolkit-Docs release archive.
