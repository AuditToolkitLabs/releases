# Switch Exposure Center

Switch-focused CMDB and exposure reporting platform for network switches and related equipment.

## Purpose

This project is a narrowed derivative of the Asset Command Center model.
It is intended to collect inventory and security exposure data from network
switches and adjacent network equipment using API, NETCONF, SNMP, and SSH.

The primary product outcome is not generic asset inventory. It is a
switch-centered operational picture that ties device identity, firmware,
advisories, CVEs, CVSS, and remediation state together.

## Product Scope

- Switch and chassis inventory for multi-vendor network estates
- API-first collection with SSH fallback only where needed
- Advisory and CVE correlation by firmware, OS train, and platform
- CVSS, KEV, and remediation-oriented reporting
- Static web console with a similar operational look and feel to the
  source CMDB platform

## MVP Vendor Coverage

- Cisco IOS-XE and NX-OS
- Juniper JunOS
- Arista EOS
- Aruba AOS-CX
- Dell OS10

Initial live API-backed implementations are provided for Cisco, Juniper,
Arista, Aruba, and Dell.

Initial SAN-oriented connector scaffolding is now present for Brocade and
Cisco MDS, with SAN-specific fields currently carried through
`custom_attributes` until real customer payloads drive schema promotion.

## Runtime Shape

- Flask API service
- SQLAlchemy data model
- Static HTML, CSS, and JavaScript console
- Connector workers for collection and normalization
- Advisory enrichment pipeline for vendor bulletins and CVE mapping

## Advisory Source Status

- Cisco advisory refresh now pulls live PSIRT RSS data and enriches CVE coverage with CISA KEV status when that feed is reachable.
- Cisco advisory refresh can also switch to an authenticated configured feed when advisory-source settings enable account mode and supply a `feed_url` plus stored Basic or Bearer credentials.
- Direct download from vendor or related advisory sites remains supported by storing the source URL in `feed_url` and supplying any required credentials or token in advisory-source settings.
- Customer-provided private advisory access can also be supplied as a local file export, including CSV-based vendor advisory exports, by storing the file path in `feed_url` while using account mode.
- Cisco correlation still reuses curated fallback metadata for known advisories so firmware matching stays useful even when the public RSS entry lacks fixed-version detail.
- Juniper advisory refresh uses the repository's normalized fallback sample feed by default, and can now switch to an authenticated account-backed RSS feed when advisory-source settings are configured.
- Arista advisory refresh uses the repository's normalized fallback sample feed by default, and can now switch to an authenticated account-backed RSS feed when advisory-source settings are configured.
- Aruba advisory refresh uses the repository's normalized fallback sample feed by default, and can now switch to an authenticated account-backed RSS feed when advisory-source settings are configured.
- Dell advisory refresh uses the repository's normalized fallback sample feed by default, and can now switch to an authenticated account-backed RSS feed when advisory-source settings are configured.
- Brocade advisory refresh uses the repository's normalized fallback sample feed by default, and can now switch to an authenticated account-backed RSS or customer-managed file feed when advisory-source settings are configured.
- Cisco MDS advisory refresh uses the repository's normalized fallback sample feed by default, and can now switch to an authenticated account-backed RSS or customer-managed file feed when advisory-source settings are configured.
- The refresh API reports per-vendor source details and source errors so operators can see when a live source fell back to sample data.
- When customers do not provide gated advisory access, the tool must rely on public sources and public vulnerability metadata such as CVE, CVSS, CISA KEV, NVD, and other open or government data feeds that are available to the current integration path.

## Advisory Automation

- Advisory refresh automation is now configured in the application and persisted in `app_metadata`.
- Operators can enable or disable recurring refresh, choose supported vendors, set the interval in minutes, and opt into full refresh behavior.
- The production-safe execution model is an external scheduler calling the due-check endpoint instead of an in-process Flask timer.
- This keeps automation deterministic across Windows Task Scheduler, cron, containers, or orchestrators without introducing duplicate in-process workers.
- A Windows scheduler helper is provided in `scripts/run_advisory_due_check.ps1`.

## Advisory Source Accounts

- Advisory-source settings are now stored separately from switch collection credentials.
- The advisory catalog page includes a vendor access panel for Brocade, Arista, Aruba, Cisco, Cisco MDS, Dell, and Juniper so operators can prepare account-backed feeds without polluting device connector defaults.
- Vendor-gated advisory access is a customer responsibility. The customer must provide either working account credentials for the private advisory feed or direct-download URL, or a customer-managed export file that the application can read.
- Brocade account mode is now wired into refresh execution and will use the configured `feed_url` plus stored Basic or Bearer credentials when present.
- Arista account mode is now wired into refresh execution and will use the configured `feed_url` plus stored Basic or Bearer credentials when present.
- Aruba account mode is now wired into refresh execution and will use the configured `feed_url` plus stored Basic or Bearer credentials when present.
- Cisco account mode is now wired into refresh execution and will use the configured `feed_url` plus stored Basic or Bearer credentials when present.
- Cisco MDS account mode is now wired into refresh execution and will use the configured `feed_url` plus stored Basic or Bearer credentials when present.
- Dell account mode is now wired into refresh execution and will use the configured `feed_url` plus stored Basic or Bearer credentials when present.
- Juniper account mode is now wired into refresh execution and will use the configured `feed_url` plus stored Basic or Bearer credentials when present.
- Those account-mode feeds may be direct downloads from vendor or related advisory sites when the customer provides the correct access path and secrets.
- Account mode can also consume a customer-supplied local RSS, XML, or CSV file path when a direct private feed cannot be accessed from the application runtime.
- Secret values remain write-only and are surfaced back only as presence flags.
- Starter customer-file templates for SAN advisory imports are provided in `docs/examples/brocade-advisories.csv` and `docs/examples/cisco-mds-advisories.csv`.

## Repository Layout

- `docs/ARCHITECTURE.md` - system design and collection flow
- `docs/OPERATIONS.md` - scheduler and runtime operating guidance
- `docs/DEPLOYMENT.md` - deployment baseline, health checks, and runtime contract
- `docs/BACKUP-RESTORE-RUNBOOK.md` - executable backup and restore drill runbook and evidence model
- `docs/DATA-MODEL.md` - canonical device and vulnerability model
- `docs/API-CONTRACTS.md` - API and collection payload contracts
- `docs/ADVISORY-FEED-VALIDATION.md` - live vendor advisory validation checklist and evidence guide
- `docs/TRANSPORT-FALLBACK-PLAN.md` - criteria and delivery order for deeper NETCONF, SNMP, and SSH support
- `docs/STATE-OF-PLAY.md` - current completeness, readiness, and next-step plan
- `src/app.py` - Flask application factory and route registration
- `src/api/` - initial API endpoints
- `src/connectors/` - connector abstractions and vendor stubs
- `src/models/` - initial ORM models
- `src/static/` - starter UI shell

## Near-Term Build Order

1. Expand live advisory normalization beyond Cisco so fixed-version and affected-range matching is vendor-backed instead of curated for Arista, Aruba, Dell, and Juniper.
2. Validate real Brocade, Cisco, Cisco MDS, Juniper, Arista, Aruba, and Dell account-feed semantics against production customer-access data and refine normalization if vendor feed shapes differ.
3. Add the next vendor advisory sources where customer environments require them.
4. Add SSH and NETCONF fallback flows where API coverage is incomplete.

## Production Baseline

- `GET /healthz` provides a liveness check for the Flask process.
- `GET /readyz` provides a readiness check that includes database reachability.
- `SWITCH_EXPOSURE_LOG_LEVEL` controls runtime log verbosity.
- Deployment guidance now lives in `docs/DEPLOYMENT.md`.
- The first full-release deployment target is a single Ubuntu Server LTS host.
- Release packages now bootstrap the app, its runtime dependencies, and HTTPS reverse proxying on the host.
- Packaging bootstrap now applies reverse-proxy security headers (CSP, HSTS, X-Frame-Options, X-Content-Type-Options, and Referrer-Policy).
- Release artifacts can be checksummed and verified with `python scripts/release_manifest.py generate ...` and `python scripts/release_manifest.py verify ...`.

## First Login Bootstrap

Switch Exposure Center does not ship with a hardcoded default username/password.

On first install, local login requires creating the first admin account through the
login page's first-time setup flow.

How the bootstrap token is sourced:

1. If `SWITCH_EXPOSURE_BOOTSTRAP_TOKEN` already exists in `/etc/switch-exposure-center/switch-exposure.env`, that value is used.
2. If it is missing, the package post-install script generates one automatically using `openssl rand -hex 24` and appends it to the env file.

Retrieve the token on the Linux host:

```bash
sudo grep '^SWITCH_EXPOSURE_BOOTSTRAP_TOKEN=' /etc/switch-exposure-center/switch-exposure.env
```

First login flow:

1. Open `/login`.
2. Complete the first-time setup form with admin username, admin password, and bootstrap token.
3. Submit `Create first admin`.
4. Sign in normally with the created credentials.

Operational recommendation:

- After first admin creation, rotate or remove `SWITCH_EXPOSURE_BOOTSTRAP_TOKEN` from the host env file.
- Restart services after env changes: `sudo systemctl restart switch-exposure-api switch-exposure-worker switch-exposure-scheduler`.

## Licensing Env Templates

Git-safe placeholder files are provided so teams can populate environment values
without committing secrets.

- Runtime template: `deploy/systemd/switch-exposure.env.example`
- Keygen bootstrap template: `deploy/systemd/keygen-bootstrap.env.example`

Recommended flow:

- Copy templates to local runtime files (gitignored):

```powershell
Copy-Item .\deploy\systemd\switch-exposure.env.example .\deploy\systemd\switch-exposure.env
Copy-Item .\deploy\systemd\keygen-bootstrap.env.example .\deploy\systemd\keygen-bootstrap.env
```

- Populate values in the copied files.

Canonical naming:

1. Runtime values use `SWITCH_EXPOSURE_*` names.
2. First-login local-admin bootstrap token uses `SWITCH_EXPOSURE_BOOTSTRAP_TOKEN`.
3. Keygen provisioning bootstrap token uses `KEYGEN_BOOTSTRAP_ADMIN_TOKEN`.
4. Stripe owns purchase and payment state; deployed runtime uses the issued Keygen license key and does not use a Keygen runtime API token.

- Load bootstrap env vars into the current shell and run provisioning:

```powershell
Get-Content .\deploy\systemd\keygen-bootstrap.env | ForEach-Object {
  if ($_ -match '^(\s*#|\s*$)') { return }
  $parts = $_.Split('=', 2)
  if ($parts.Count -eq 2) { Set-Item -Path ("Env:" + $parts[0].Trim()) -Value $parts[1].Trim() }
}

pwsh -NoProfile -ExecutionPolicy Bypass -File .\scripts\bootstrap_keygen_licensing.ps1 -DryRun
```

### GitHub CLI Sync (Repo Variables And Secrets)

You can publish licensing placeholders and values directly to GitHub Actions
variables and secrets with GitHub CLI.

Dry-run preview:

```powershell
pwsh -NoProfile -ExecutionPolicy Bypass -File .\scripts\configure_github_keygen_vars.ps1
```

Apply to repository scope:

```powershell
pwsh -NoProfile -ExecutionPolicy Bypass -File .\scripts\configure_github_keygen_vars.ps1 -Apply -UsePlaceholderSecrets
```

Apply to a specific GitHub Actions environment, for example staging:

```powershell
pwsh -NoProfile -ExecutionPolicy Bypass -File .\scripts\configure_github_keygen_vars.ps1 -Apply -EnvironmentName staging -UsePlaceholderSecrets
```

## Release Flow

For lab and release-host validation, build and install the Debian package, then run the release validation helper:

```powershell
pwsh -NoProfile -ExecutionPolicy Bypass -File .\scripts\run_release_validation.ps1
```

That helper verifies SSH access, installed package state, nginx, and HTTPS health end to end.

For release artifact integrity checks:

```bash
python scripts/release_manifest.py generate --version 0.1.0 --output dist/release-manifest.json dist/switch-exposure-center_0.1.0_all.deb
python scripts/release_manifest.py verify --manifest dist/release-manifest.json
```

## Development

This repo now includes working multi-vendor inventory collection, persisted
connector settings, advisory refresh, catalog correlation, and strict test
coverage. The remaining work is mostly about replacing fallback data paths,
adding deeper collection transports, and hardening operator workflows.
