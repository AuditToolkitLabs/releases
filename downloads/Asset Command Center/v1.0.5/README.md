# Asset Command Centre

This repository is centered on the legacy-focused Asset Command Centre
service in `tools/asset-management`.

Repository slug remains `CMDB-Full-Asset-Platform` during migration for
compatibility with existing release, packaging, and automation contracts.

## Planned Release Version

Planned release version for the next customer publication set: 1.0.5.

Security evidence for this planned release is maintained in:

- `SECURITY.md`
- `SECURITY_AUDIT_REPORT.md`
- `RELEASE_READY_REPORT.md`
- `customer-docs/asset-command-center/14-customer-security-assurance-report.md`

## Documentation Layout

Customer-facing material now lives under `customer-docs/`:

- `customer-docs/README.md`
- `customer-docs/asset-command-center/README.md`
- `customer-docs/asset-command-center/playbooks/discovery/README.md`
- `customer-docs/legal/EULA.md`

Internal-only material now lives under `docs/`:

- `docs/TOOLKIT-ARCHITECTURE-WAY-AHEAD.md`
- `docs/RELEASE-SIGNING.md`
- `docs/asset-command-center/CONNECTIVITY-ARCHITECTURE-OVERVIEW.md`
- `docs/asset-command-center/IMPLEMENTATION-PROGRESS.md`

## Active Service

Primary implementation path:

- `tools/asset-management`

Primary startup entry point for local development:

- `tools/asset-management/run_local.py`

## Windows Installer Variants

Release packaging now publishes two Windows MSI variants:

- `cmdb-platform_<version>_win_x64.msi`
  Offline/full installer. Bundles heavyweight prerequisites (larger download).
- `cmdb-platform_<version>_win_x64_online.msi`
  Online installer. Downloads prerequisites during install (smaller download).

Use the offline MSI for restricted or air-gapped environments.
Use the online MSI when outbound internet access is available.
