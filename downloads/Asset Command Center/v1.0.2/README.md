# Audit Tool CMDB Asset Platform

This repository is centered on the legacy-focused Asset Command Center service in `tools/asset-management`.

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

- `cmdb-platform_<version>_win_x64.msi` Offline/full installer. Bundles heavyweight prerequisites (larger download).
- `cmdb-platform_<version>_win_x64_online.msi` Online installer. Downloads prerequisites during install (smaller download).

Use the offline MSI for restricted or air-gapped environments. Use the online MSI when outbound internet access is available.
