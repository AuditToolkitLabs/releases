# Changelog

<!-- markdownlint-disable MD013 -->
<!-- Line length rule disabled - changelog entries benefit from single-line readability -->

All notable changes to this project will be documented in this file.

## [1.2.2] - 2026-05-19 (Release Baseline Alignment)

### Changed

- **Release sequencing fix:** aligned repository metadata, legal/customer/security documentation, and release-note artifact references to `1.2.2` before publishing the release tag.
- **Customer installation references:** updated package names and release-download URLs to `v1.2.2` / `1.2.2` across customer installation and transition guidance.
- **Assurance references:** refreshed OWASP scorecards and the customer security/quality assurance report to assess `v1.2.2`.

## [1.2.1] - 2026-05-19 (Release Hotfix and Assurance Refresh)

### Fixed

- **Windows developer web path resolution:** fixed Linux audit discovery in `agents/html-linux/web/app.py` for Windows repo runs by preferring the checked-out repository audit path when present.

### Changed

- **Release baseline:** advanced repository release metadata to `1.2.1` in `VERSION`.
- **Customer documentation sync:** updated customer-facing release references across installation, transition, operations, legal, and assurance documents to `v1.2.1`.
- **Legal and policy metadata refresh:** aligned release-reference fields in legal/policy documents (EULA, privacy policy, disclaimer, and customer legal summary) with `v1.2.1`.
- **Security assurance refresh:** updated OWASP scorecards and the customer security/quality assurance report to reference `v1.2.1` and include hotfix-cycle evidence.

## [1.2.0] - 2026-05-18 (Release Baseline and Documentation Alignment)

### Changed

- **Release version baseline:** advanced repository release metadata to `1.2.0` in `VERSION`.
- **Customer documentation sync:** updated customer-facing release references across the customer-docs set to `v1.2.0` for the planned release cycle.
- **Legal and policy metadata refresh:** aligned release-reference fields in legal/policy documents with `v1.2.0`.
- **Security assurance documentation refresh:** updated OWASP scorecards and release automation guidance to reference `v1.2.0` as the active target release.

## [1.1.8] - 2026-05-15 (Release Documentation and Legal Sync)

### Changed

- **Release version baseline:** advanced the repository release version to `1.1.8` in `VERSION` and aligned customer-facing release references accordingly.
- **Customer documentation release sync:** updated installation, transition, operations, index, assurance, and screenshot-manifest customer docs to reference `v1.1.8` as the current release.
- **Legal and policy metadata refresh:** updated release-reference metadata in legal-facing files to match the 1.1.8 release cycle.
- **Security assurance evidence refresh:** updated the OWASP scorecards and customer security/quality assurance report so they explicitly describe the published `v1.1.8` release posture.

## [1.1.6] - 2026-05-15 (Release Version and Customer Assurance Sync)

### Changed

- **Release version baseline:** advanced the repository release version to `1.1.6` in `VERSION` and aligned customer-facing release references accordingly.
- **Customer documentation release sync:** updated installation, transition, operations, index, and legal-summary customer docs to reference `v1.1.6` as the current release.
- **Legal and policy metadata refresh:** updated release-reference metadata and last-updated timestamps in legal-facing files to match the 1.1.6 release cycle.

### Added

- **Customer security assurance report:** added `customer-docs/25-security-and-quality-assurance-report.md` with deployment-focused security and quality evidence for on-site customer assurance.

## [1.1.4] - 2026-05-05 (Release Sync and LFS Cleanup)

### Fixed

- **Updater reliability under strict bash mode:** corrected `compare_versions` call handling in `scripts/toolkit-updater.sh` so semantic non-zero returns are captured safely under `set -euo pipefail` instead of aborting update/patch flows.
- **Phase 7 version-gate smoke pathing:** `ci/tests/test-upgrade-preserves-state.sh` now writes the smoke test target version into the staging path used by the updater gate logic.

### Added

- **Upgrade lifecycle hardening controls:** added downgrade/re-install gating defaults and post-upgrade wrapper health verification with automatic rollback guardrails.
- **Install/rollback safety improvements:** partial-install cleanup trap, `SKIP_BACKUP` sentinel behavior, and root checks before package-manager operations for `.deb`/`.rpm` flows.

### Changed

- **Git LFS decommissioning for release flow:** removed repository LFS tracking rules as release artifacts are now produced via GitHub Actions release jobs, eliminating local `git-lfs` hook dependency warnings during normal development.
- **Customer documentation release sync:** updated customer docs and release references to `v1.1.4` so published documentation matches shipped artifacts.

## [1.1.3] - 2026-05-05 (Upgrade Safety Hardening Release)

### Fixed

- **Package identity mismatch:** `scripts/toolkit-updater.sh` was querying and upgrading `audit-tool` instead of the actual package name `audit-toolkit-lite`. Corrected in both the detection paths (`dpkg -l`, `rpm -q`) and the apply paths (`apt-get install --only-upgrade`, `yum/dnf update`).
- **Repository identity mismatch:** `scripts/toolkit-updater.sh` and `install.sh` referenced `Audit-Tool-` as the GitHub repository slug. Corrected to `AuditToolkit-Linux-Security-Lite` across updater, installer, and uninstaller.
- **Updater omitted from release packages:** `scripts/toolkit-updater.sh` was not included in `packaging/build-packages.sh` manifest. Added to `INCLUDE_FILES`; build now creates `scripts/` subdirectory and sets executable permission on the updater.
- **Destructive source update:** `apply_source_update()` and `rollback()` in `toolkit-updater.sh` used `rm -rf` followed by `cp -a`, which risked data loss during an interrupted update. Replaced with `rsync -a --delete` where available, with an additive `cp -a .` fallback.
- **DEB `prerm` broke upgrades:** `packaging/deb/DEBIAN/prerm` unconditionally removed `/usr/local/bin/audit-toolkit` before any dpkg action, including upgrades. Wrapped the removal in `case "$1" in remove|purge)` so the wrapper is preserved during `upgrade` and `failed-upgrade` transitions.
- **`install.sh` forced wipe on re-run:** `clone_repository()` always deleted and re-cloned the install directory. Now attempts `git pull --ff-only` for existing git-based installs and only prompts for full reinstall if fast-forward fails or the existing install is not a git repository.
- **Reinstall hint URL in `uninstall.sh`:** Corrected `Audit-Tool-` → `AuditToolkit-Linux-Security-Lite` in the reinstall curl command printed at teardown.

### Added

- **CI regression test** `ci/tests/test-upgrade-preserves-state.sh`: 10-assertion test suite enforcing all upgrade-safety invariants — package/repo identity in updater and packaging scripts, conditional `prerm` behaviour, and a live patch-apply smoke test verifying that customer config (`/etc/audit-toolkit/`) and logs (`/var/log/audit-toolkit/`) survive a patch cycle.
- **Pre-release safety gate** in `.github/workflows/release-packages.yml`: new `pre-release-gate` job runs the full Phase 7 test suite (including upgrade-safety) before the `build-packages` job is allowed to produce release artifacts.

### Changed

- `ci/run-phase7-tests.sh` now includes `Upgrade Preserves State` as the fifth registered test suite.

### Upgrade Notes

No schema changes. No breaking changes to audit output or report format. `scripts/toolkit-updater.sh` is now included in all release packages (tarball, `.deb`, `.rpm`) — previously it was only available in source checkouts. Existing installations upgraded via `.deb` or `.rpm` will gain the updater automatically.

---

## [1.1.1] - 2026-05-03 (Signed Security Documentation Release)

### Added
- OWASP security scorecard at `docs/OWASP-SECURITY-SCORECARD.md`
- Customer-facing copy at `customer-docs/OWASP-SECURITY-SCORECARD.md`

### Changed
- OWASP scorecard updated to `100/100 (A+)` and A08 marked fully closed (GPG signing enforced for commits and release artifacts)
- `SECURITY.md` updated to reflect latest security posture and scorecard alignment
- Linked scorecard across docs and customer docs indexes/navigation

### Fixed
- Resolved markdownlint violations in `customer-docs/24-enterprise-compliance-platform.md`
- Added `.gitignore` rules for local underscore-prefixed scan/lint/test artifact files (`_bandit_*`, `_flake8_*`, `_pytest_*`, `_full_*`)

## [1.0.0] - 2026-04-30 (Purpose Alignment Release)

### Purpose-Aligned Implementation Complete

All 8 phases of the purpose-alignment roadmap are complete. The toolkit now provides standardized, confidence-scored, multi-distro audit reports suitable for compliance gates and SIEM integration.

### Phase 1: Unified Data Model ✅
- JSON Schema v1.0 (`schema/audit-report.v1.schema.json`) defines normalized report structure
- Commit: [ac95ebc](https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/commit/ac95ebc)

### Phase 2: Deterministic Inventory ✅
- `lib/inventory.sh` provides multi-distro package/service/port detection
- Inventory confidence scoring (high/medium/low)
- Commit: [490d867](https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/commit/490d867)

### Phase 3: Unified Vulnerability Correlation ✅
- `lib/vuln.sh` abstracts CVE detection across package managers
- Tiered evidence: Tier A (CVE+severity), Tier B (advisory), Tier C (pending update)
- Commit: [19a8faf](https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/commit/19a8faf)

### Phase 4: Hardening Control Baseline Contract ✅
- 29 audit scripts annotated with `CONTROL_ID` and `CONTROL_TIER`
- 12 required + 17 recommended baseline controls
- `HARDENING-CONTROL-BASELINE.md` catalogue with CIS/NIST/PCI/STIG mappings
- `ci/validate-control-baseline.py` ensures no orphaned controls
- Commit: [9bf3bef](https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/commit/9bf3bef)

### Phase 5: Installed-Stack to Audit Auto-Plan ✅
- `orchestrator.sh --auto` detects installed software and recommends audits
- `discovery.sh` maps software to applicable audit scripts
- Skipped domains tracked in completeness metadata
- Commit: [43c3f57](https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/commit/43c3f57)

### Phase 6: Completeness and Confidence Reporting ✅
- `completeness.confidence_score` (0-100%) indicates audit coverage
- `completeness.skip_reasons` structured as `{insufficient_privilege, missing_tool, not_applicable, unsupported_distro_path}`
- Required vs. optional check accounting for baseline vs. recommended controls
- Commit: [9296794](https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/commit/9296794)

### Phase 7: CI Verification for Purpose ✅
- `ci/test-assertions.sh` framework for JSON validation
- 4 test suites: inventory, hardening, completeness, full pipeline
- `.github/workflows/ci.yml` `purpose-test` job validates schema and completeness
- Commit: [5fd8fe7](https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/commit/5fd8fe7)

### Phase 8: Operational Rollout and Governance ✅
- `docs/OPERATOR-RUNBOOK.md` — Deployment, scheduling, and remediation guide
- `docs/REPORT-CONSUMER-GUIDE.md` — Complete schema field reference for SIEM/dashboard integration
- Versioning policy: schema v1.0 is stable; breaking changes require version bump
- CI gates and compliance scoring enable automated deployment pipelines

### Added

- **Documentation (Phase 8)**
  - OPERATOR-RUNBOOK.md: systemd timer, cron, and remediation examples
  - REPORT-CONSUMER-GUIDE.md: Complete schema reference with integration examples
  - Updated PURPOSE-ALIGNMENT-IMPLEMENTATION-CHECKLIST.md with all phase completions

- **CI Improvements (Phase 7)**
  - Test assertion framework with JSON validation helpers
  - Phase 7 test suite: inventory, hardening, completeness, end-to-end validation
  - New CI job: `purpose-test` for continuous purpose-alignment verification

- **Completeness Metrics (Phase 6)**
  - Confidence and coverage scores in all reports
  - Structured skip reasons for audit gaps
  - Required/optional check accounting

### Changed

- Schema v1.0 is now production-stable (locked for all Phase 1-8 implementations)
- All 8 phases marked complete in PURPOSE-ALIGNMENT-IMPLEMENTATION-CHECKLIST.md

### Known Limitations

- NixOS and dinit init systems: Not yet supported (Phase 8+ roadmap)
- Alpine Linux: Reduced audit coverage due to minimal base image (see LINUX-DISTRO-COVERAGE-MATRIX.md)

### Upgrade Notes

Version 1.0.0 introduces no breaking changes from schema v1.0 (introduced in Phase 1). Existing report parsers remain compatible.

---

## [1.0.1] - 2026-05-01

### Fixed

- **ShellCheck SC2009:** Replaced `ps|grep` process-detection patterns with `pgrep`-based checks in `audits/linux/network/dns/dns-security-audit.sh`.
- **ShellCheck SC2154/SC2016:** Resolved `$FileCreateMode` regex literal-token handling in `audits/linux/advanced-controls/logging-audit.sh` using POSIX character class `[$]`.
- **ShellCheck SC1091:** Fixed include-path source hints in `audits/linux/network/firewall/firewall-state.sh` (repository-root-relative hints) and `ci/bootstrap.sh` (scoped disable for `/etc/os-release`).
- **ShellCheck SC1091:** Corrected include-path hints in `audits/linux/platform/access/ssh-hardening-audit.sh`, `audits/linux/platform/baseline/svc-basics.sh`, `audits/linux/platform/baseline/updates.sh`.

### Changed

- **OWASP Scorecard:** Updated `docs/OWASP-SECURITY-SCORECARD.md` with May 2026 revalidation addendum; split A08 integrity control into _Commit Signing (Git)_ ✅ and _Release Artifact Signing_ ⚠️.
- **Assessment Date:** Advanced to 2026-05-01.

### Validated

- `shellcheck --severity=warning` passes on all modified shell scripts.
- All commits on `main` are GPG-signed (key `926C3CBBB1CC2C973891206B1AA413CA5B8F5229`).
- OWASP score remains **97/100 (A+)**.

---

## [5.6.1-beta] - 2026-03-04 (updated 2026-03-21)

### Security Hardening & Release Stabilization

This beta release rolls forward recent hardening, release hygiene, and cross-platform validation work into a single release target.

### Added

- **Release Documentation Updates**
  - Added release notes for `v5.6.1-beta`
  - Updated public version surfaces in top-level docs

### Changed

- **Asset Discovery Security Hardening**
  - Replaced subprocess-based host reachability probing in `remote_executor.py` with socket-based probing to reduce command execution surface
  - Hardened command execution and dynamic SQL handling paths in asset discovery service modules

- **Documentation & Lint Hygiene**
  - Normalized markdown formatting and rule compliance across release and operational docs
  - Added targeted markdown lint ignore patterns for generated evidence/report artifacts

### Fixed

- **Static Security Scan Findings**
  - Resolved remaining production-scope Bandit medium/high findings in `web` and `tools/asset-discovery/src`
  - Cleared final production-scope low finding associated with subprocess import usage

- **Release Consistency**
  - Aligned toolkit version markers to `5.6.1-beta` across active release-facing documentation

### Validation

- Production-scope Bandit verification report (`web` + `tools/asset-discovery/src`) now returns `0` findings in latest rerun
- Medium/high quality gate (`bandit -ll`) passes for production scope

### Hotfix — Package Contents & Appliance Artifacts (2026-03-21)

#### Fixed

- **Linux Package Contents**
  - `Build-DEB.sh` and `Build-RPM.sh` updated to include `scripts/` directory (audit scripts, lifecycle helpers, setup helpers)
  - `Build-DEB.sh` and `Build-RPM.sh` updated to include `deployment/windows/jea/Invoke-SecurityAuditJEA.ps1` (required by deploy page JEA download)
  - Packages rebuild verified: DEB 2,634 files, RPM 2,629 files

- **Windows MSI Package Contents**
  - `SecurityAuditToolkit.wxs` updated to include `ScriptsDir` component group packaging all Bash/PowerShell audit and lifecycle scripts
  - `SecurityAuditToolkit.wxs` updated to include `DeploymentWindowsJeaDir` component for JEA deployment script
  - Three deploy-page installer scripts that were missing from prior MSI builds are now included

#### Added

- **VM Appliance Distribution — 5.6.1-beta**
  - OVA and VMDK split archives added under `deployment/vm-appliance/output/` as Git LFS objects
  - OVA split: `security-audit-toolkit-5.6.1-beta.ova.part001`–`part007` + `parts.json` (7 × ~900 MB parts)
  - VMDK split: `security-audit-toolkit-5.6.1-beta.vmdk.part001`–`part007` + `parts.json`
  - Total appliance size: OVA 2.73 GB / VMDK 2.79 GB
  - SHA-256 checksums documented in `APPLIANCE-RELEASE-5.6.1-BETA.md`

- **Build & Release Tooling**
  - `tools/run-git-lfs-push-detached.ps1` — resilient detached LFS push worker with Start/Stop/Status/Worker modes
  - `tools/run-msi-build-detached.ps1` — detached MSI build helper
  - `tools/start-msi-build-admin.ps1` — elevated MSI build launcher

## [5.3.0-beta] - 2026-02-25

### Appliance Distribution & Password Security Framework

Major feature release adding enterprise VM appliance distribution (OVA/OVF/VHDX), cryptographic password security layer, comprehensive maintenance automation, and appliance lifecycle documentation.

### Added

- **Virtual Appliance Distribution System**
  - OVA/OVF/VHDX export formats from PowerShell build tooling
  - Non-VMware OVF/OVA packaging path added (manual OVF + manifest + tar archive)
  - New packager script: `tools/package-ova-from-vmdk.ps1`
  - Deployment Center download-only distribution for appliance formats
  - External URL support via environment variables (`VM_APPLIANCE_OVA_URL`, `VM_APPLIANCE_OVF_URL`)
  - `/api/deploy/package-status` endpoint for artifact availability detection
  - Rebuild helper script (`tools/rebuild-vm-appliance.ps1`) for admin self-service builds
  - Rebuild helper now auto-invokes local OVF/OVA packager after appliance build
  - Download button state management based on artifact availability

- **Auto-Generated Password Security Layer**
  - User creation: Auto-generated 16-character passwords (read-only field prevents override)
  - Password reset: Auto-generated 16-character passwords with show/hide and copy controls
  - Password regenerate button for alternative generation
  - Cryptographic randomness with guaranteed complexity (4 character types)
  - One-click clipboard copy with visual feedback
  - Automatic `must_change_password=true` flag for forced user reset
  - Success alert displays password one final time after account creation
  - Edit user modal hides password section (use reset function instead)

- **Maintenance & Patch Automation**
  - Automatic security updates enabled by default (`unattended-upgrades` package)
  - Initial system patch applied during first appliance boot
  - Daily update checks configured
  - Configurable automatic reboot policy (disabled by default)
  - `/var/log/unattended-upgrades/` log tracking

- **Comprehensive Operator Documentation**
  - `deployment/vm-appliance/MAINTENANCE-REFERENCE.md` - Daily/weekly/monthly operational tasks
  - Enhanced `OVA-BUILD-GUIDE.md` with maintenance workflow
  - Enhanced `APPLIANCE-SECURITY-GUIDE.md` with update policies
  - Legacy v5.2.0 migration/validation docs were produced in-cycle and later pruned during v5.6.1-beta cleanup
  - `PASSWORD-RESET-IMPROVEMENTS.md` - Summary of password management enhancements
  - `USER-CREATION-AUTO-PASSWORD-FEATURE.md` - User provisioning security details
  - `ADMIN-PASSWORD-RESET-FEATURE.md` - Password reset endpoint & UI documentation

### Changed

- **Deployment Center UI**
  - Restored "Full Deployment" tab (download-only, no remote push)
  - Separated "Agent Deployment" tab (supports remote push)
  - Virtual Appliance card moved to download-only section
  - 14 contextual help buttons (?) added across all deployment options
  - Help modals provide use-case guidance and prerequisites
  - OVA/OVF buttons gated by `/api/deploy/package-status` response
  - Download-only messaging clarified for appliance artifacts

- **Tab Labeling**: "Tooling" → "Agent Tooling" for clarity

- **Standalone Helpers Card**: Added Virtual Appliance Artifacts section with rebuild-vm-appliance.ps1 download

### Fixed

- All issues from 5.2.0-beta remain fixed; no regressions introduced

### Testing & Validation

- **Docker Fresh Build**: All 4 web service images rebuilt (no cache) successfully
- **Service Health**: 6/6 containers running (web, asset-discovery, celery-worker, celery-beat, PostgreSQL, Redis)
- **API Endpoints**: All 12 routes active; 1 new endpoint (`/api/deploy/package-status`) operational
- **Password Generation**: 10/10 manual tests passed (auto-gen, regenerate, show/hide, copy, create, reset)
- **VM Appliance**: Artifact availability detection verified
- **Backward Compatibility**: Zero breaking changes; v5.2.0 → v5.3.0 migration path verified

### Security Enhancements

- **Password Generation**: 16-character cryptographic random (4 character type guarantee)
- **No Manual Passwords**: Admins cannot set weak initial passwords during user creation
- **Forced Reset**: All new users must change temporary password on first login
- **Audit Trail**: Full logging of user creation and password reset events
- **LDAP Protection**: Non-local accounts (LDAP/AD/Entra/SAML/OIDC) cannot be reset via UI

### Known Limitations

- OVA/OVF artifacts must be provided externally via release URLs if repo size is a constraint
- Auto-reboot disabled by default; configure in `/etc/apt/apt.conf.d/50unattended-upgrades` if desired
- Appliance maintenance tasks remain manual operator responsibility (no auto-remediation)

## [5.2.0-beta] - 2026-02-23

### Beta Release - Enterprise Hardening & Deployment Framework

First public Beta release featuring environment profiles, secret automation, comprehensive enterprise documentation, and transparent Beta program framework with phased support model.

### Added

- **Break-Glass Recovery Admin Account**
  - `is_builtin_admin` flag for SuperAdmin recovery account
  - Break-glass account always uses local authentication (bypasses SSO/LDAP/Entra)
  - Ensures operators can always recover system access like Linux `root`
  - Migration `013_add_builtin_admin_flag` marks first local SuperAdmin as break-glass
  - Database migration auto-marks existing admin user on upgrade

- **Environment Profile System**
  - `ENV_PROFILE` support: `dev`, `staging`, `preprod`, `prod`
  - Profile-aware certificate generation (development vs. production)
  - Deployment guide documentation with multi-environment strategies
  - Automated profile detection in deployment scripts

- **Automated Secret Management**
  - `.env.secrets.generation.log` automatic generation during deployment
  - Pre-generated secrets for Flask, JWT, API encryption, license signing
  - Secure random generation with cryptographic strength
  - Integration with `deploy.sh` and Docker Compose workflows

- **Enterprise Documentation Suite**
  - `ENTERPRISE-GO-LIVE-GATE-REVIEW-2026-02-23.md` - 8-gate readiness framework
  - `ENTERPRISE-SUPPORT-MODEL-DECISION-GUIDE.md` - 3-path support framework (community/custom/waitlist)
  - `BETA-READINESS-ASSESSMENT.md` - Comprehensive pre-launch evaluation (97/100 security score)
  - `BETA-PROGRAM-TERMS.md` - Legal framework for Beta participants
  - Legacy v5.2.0 version-consistency checklist was produced in-cycle and later pruned during v5.6.1-beta cleanup

- **Beta Program Framework**
  - Transparent sole-developer model disclosure
  - 3-tier support paths: Path A (GitHub community), Path B (custom $100K-$250K), Path C (waitlist)
  - Known limitations documentation with platform edge cases
  - Acknowledgment checkboxes for participant responsibilities

- **Version Consistency**
  - Unified version across 58 files (agents, web Python, HTML, deployment, Docker, docs)
  - Deployment package builders updated: MSI, DEB, RPM, Docker, VM appliance
  - Agent version reporting standardized: `agent.sh`, `agent.ps1`, `managed-agent.sh`, `managed-agent.ps1`
  - Web UI version badges updated to orange "Beta" status

- **Security Enhancements**
  - Fernet key validation extended to deployment secret generation
  - Production hardening checklist integrated with deployment guide
  - 100/100 test pass rate maintained (297 tests across unit/integration/security)
  - 97/100 OWASP score (A+) with 0 HIGH/MEDIUM Bandit findings
  - Windows `.env` file ACL protection in `setup.ps1` (parity with Linux `chmod 600`)

### Fixed

- **Docker Compose Celery Workers** - Added missing `PRODUCTION_HARDENING_REQUIRED` and `DB_FIELD_ENCRYPTION_KEY` environment variables to celery-worker and celery-beat services
- **Docker Compose Production** - Aligned `docker-compose.prod.yml` with current codebase: `app.celery` → `celery_app`, `SECRET_KEY` → `FLASK_SECRET_KEY`, `audit_dashboard` → `audit_toolkit`, added working_dir
- **Asset Discovery Navigation** - Fixed Console and Reports shortcuts going to home page instead of `/console#execution` and `/console#reports`
- **LDAP/Entra Config Routes** - Added missing LDAP and Entra ID configuration API routes, fixed auth on settings/logs endpoints
- **Asset Discovery Packaging** - Included asset-discovery scripts in DEB/RPM packages
- **Asset Discovery Enablement** - Ensured asset-discovery is enabled in all environment files
- **CVE Records Migration** - Auto-migrate missing `cve_records` columns to prevent CVE load failures

### Changed

- **Support Model**: Clarified sole-developer limitations, established phased rollout framework
- **README.md**: Added dual badges - Version 5.2.0-beta (orange) + Status: Beta (orange)
- **Deployment Guides**: Updated all package build examples with 5.2.0-beta versions
- **Integration Testing**: Updated `INTEGRATION_TESTING_QUICK_START.md` for Beta version
- **BSL 1.1 License**: Maintained with 25-machine free tier (perpetual)

### Testing & Validation

- **97/100 Security Score** (A+ rating):
  - HTTPS/TLS enforcement (A01, A02)
  - Input validation & sanitization (A03)
  - RBAC with role hierarchy (A01)
  - Secrets management with Fernet encryption
  - CSRF protection, secure session management

- **Test Coverage**:
  - 297 audit scripts (138 Linux, 147 Windows, 12 hypervisors)
  - 100% API test pass rate
  - Agent heartbeat validation passing
  - License tier enforcement tested

### Known Limitations (Beta)

- **Sole Developer Model**: Single maintainer, limited support capacity
- **Platform Edge Cases**: Some distros/hypervisors may require customization
- **Performance**: Not yet optimized for >1000 concurrent agents
- **Enterprise Integration**: SIEM, ticketing, cloud provider APIs in early stage
- **Documentation**: Some advanced deployment scenarios under-documented

### Breaking Changes

None - fully backward compatible with 5.1.5 for existing deployments.

---

## [ARCHIVED - UNRELEASED] Below entries (5.3.1, 5.3.0, 5.2.0) were documented but never tagged/released. Preserved for reference only.

## [5.3.1] - 2026-02-21 [UNRELEASED]

### Phase 3 Request-Signature Hardening (Managed + Standalone Parity)

Security hardening update that completes request-signature enforcement coverage
for managed command lifecycle routes and adds configuration/operational parity
across core admin settings, managed agent paths, and standalone agents.

### Added

- **Managed Request-Signature Enforcement on Command Lifecycle**
  - Added Phase 3 signature checks to command routes:
    - `GET /api/agent/commands`
    - `POST /api/agent/commands/<id>/ack`
    - `POST /api/agent/commands/<id>/start`
    - `POST /api/agent/commands/<id>/complete`
  - Validation includes timestamp freshness, payload hash integrity,
    and HMAC signature verification.

- **Admin Security Configuration Parity**
  - Exposed managed request-signature controls in Agent Security admin UI:
    - enforce signatures toggle
    - max-age setting
    - request signing key

- **Standalone Agent Configuration + Runtime Parity**
  - Added standalone web settings fields for managed request-signature controls.
  - Added standalone CLI runtime support for managed request-signature headers:
    - Bash (`lightweight-agent/agent.sh`)
    - PowerShell (`lightweight-agent/agent.ps1`)
  - Signature headers now emitted by shared request wrappers when enabled:
    - `X-Agent-Timestamp`
    - `X-Agent-Content-SHA256`
    - `X-Agent-Signature`

- **Regression Coverage**
  - New regression test:
    - `tests/test_phase3_standalone_cli_request_signature_wiring.py`
  - Verifies standalone CLI wiring remains present for both Bash and
    PowerShell agents.

### Testing & Validation

- Combined Phase 3 signature gate passed:
  - `python -m unittest tests.test_phase3_command_request_signatures`
    `tests.test_phase3_managed_request_signatures`
    `tests.test_phase3_standalone_cli_request_signature_wiring -v`
  - Result: `10 tests passed`
- Additional script validation:
  - `bash -n lightweight-agent/agent.sh`
  - PowerShell parse check passed for `lightweight-agent/agent.ps1`

### Security (Dependency Updates)

- **Flask 3.1.3**: Fixes potential security issues in older versions
- **Werkzeug 3.1.6**: Security patches for WSGI utilities
- **Removed nltk**: Eliminated unused dependency with unpatched CVE (no
  upstream fix available)

### Operational Improvements

- **Heartbeat Alerting Dashboard**: Added "Agents Needing Attention" panel
  to Admin → Agents page:
  - Displays offline agents (no heartbeat in 10+ minutes)
  - Highlights degraded agents (connectivity issues)
  - Shows agents with export failures

- **Strict Mode Operator Runbook**: Added comprehensive step-by-step procedure
  for enabling request signature enforcement:
  - Phase 1: Compatibility window (monitor only)
  - Phase 2: Canary enforcement
  - Phase 3: Tighten freshness window
  - Phase 4: Production steady state
  - Quick reference commands for troubleshooting

### Code Quality

- Fixed unused imports across Python modules (auth_routes, capture-screenshots)
- Fixed ambiguous variable names in hypervisor platforms (proxmox, esxi)
- Added shellcheck disable comments for intentionally-reserved shell variables
- Fixed markdown lint warnings in operator runbook and hardening checklist

## [5.3.0] - 2026-02-20

### Management Agent Heartbeat Validation (v5.3.0 Pre-Phase 3)

Major enhancement to Management Agent architecture with strict heartbeat validation and license enforcement before Phase 3 JAR consolidation.

### Added

- **Heartbeat Validation Framework**
  - Strict heartbeat validation on every polling cycle (not just startup)
  - Server-enforced license validation (cannot be bypassed offline)
  - Fail-safe hard stop after 3 consecutive heartbeat failures
  - Agent exits with code 2 on license expiration or connectivity loss

- **Database Schema (Management Agent Heartbeat Tracking)**
  - `last_heartbeat_at`: Timestamp of last successful heartbeat
  - `consecutive_heartbeat_failures`: Failure counter (0-3) for hard-stop logic
  - `last_heartbeat_status`: Status code (ok|invalid_key|license_expired|connection_error)
  - `heartbeat_failure_reason`: Human-readable failure explanation
  - 3 new indexes for dashboard query optimization

- **Core Server API Endpoints**
  - `GET /api/managed-agent/heartbeat` (v5.3.0): Validates agent heartbeat, checks license, updates DB
  - `GET /api/managed-agent/health` (v5.3.0): Dashboard endpoint for agent connectivity status
  - Enhanced `POST /api/managed-agent/results`: Accepts connectivity metadata in payload

- **Agent Connectivity Reporting**
  - Agents report connectivity status (heartbeat_at, failures, reasons) with each result
  - Dashboard shows: Active (< 5 min) | Dark (5-30 min) | Down (> 30 min) status
  - License status displayed per agent (Active | Expired | Invalid)
  - Failure tracking visible to operations team

- **Management Agent Scripts (v5.3.0)**
  - `managed-agent-v5.3.0.sh` (Bash): Heartbeat-gated polling loop with fail-safe logic (440 lines)
  - `managed-agent-v5.3.0.ps1` (PowerShell): Windows equivalent with feature parity (410 lines)
  - Both versions implement strict heartbeat validation, license re-checking, and graceful shutdown

- **Database Migration (011_add_heartbeat_tracking_v5_3.py)**
  - Alembic migration with upgrade/downgrade support
  - Backward compatible (all new columns nullable with defaults)
  - Can be rolled back if needed without data loss

### Changed

- **Management Agent Polling Architecture**
  - Old: Heartbeat validated only at startup
  - New: Heartbeat validated before every command polling cycle
  - Old: Client-side license validation (can work offline)
  - New: Server-enforced license validation (cannot execute without server approval)
  - Old: Continues polling on connection errors indefinitely
  - New: Hard stop (exit 2) after 3 consecutive failures

- **Agent Control**
  - Operators can now revoke licenses immediately (next heartbeat fails)
  - No agent restart needed for license updates
  - Centralized heartbeat state tracking in database

### Better DevOps Observability

- Dashboard shows real-time agent health status
- Failure reasons visible for troubleshooting
- Connectivity metadata in audit results for traceability
- Database indexes optimize dashboard queries

### Backward Compatibility

- v5.3.0 agents understand v5.2.0 message format
- v5.2.0 agents continue to work (no forced upgrade)
- v5.3.0 Core Server validates both v5.2.0 and v5.3.0 agents
- Safe for mixed deployment during rollout

### Pre-Phase 3 Benefits

- Proven heartbeat pattern before Phase 3 JAR consolidation
- Operators familiar with fail-safe behavior in production
- De-risks Phase 3 implementation (patterns already validated)
- Foundation for Phase 3 agent licensing inside JVM

### Testing & QA

- [x] Database migration tested (upgrade/downgrade)
- [x] ORM model fields validated
- [x] API endpoints implemented
- [x] Bash and PowerShell scripts complete
- [ ] Integration testing with Core Server
- [ ] QA test scenarios (401, 403, timeout, mixed versions)
- [ ] Canary rollout (10% → 25% → 50% → 100%)

---

## [5.2.0] - 2026-02-18 [UNRELEASED]

### Audit Script Expansion

Major expansion of audit script coverage with 44 new scripts.

### Added

- **Windows Third-Party Application Audits** (39 scripts)
  - Browsers: Chrome, Firefox, Edge Chromium security audits
  - Communication: Slack, Zoom, Microsoft Teams audits
  - Password Managers: 1Password, Bitwarden, LastPass, KeePass audits
  - Cloud Storage: OneDrive, Dropbox, Google Drive audits
  - Productivity: Adobe Acrobat, 7-Zip audits
  - Many more enterprise application security audits

- **Linux Audit Enhancements** (20+ scripts with elevation tags)
  - All 138 Linux scripts now have `@elevation` tags for wrapper compatibility
  - Elevation levels: `root` (35), `partial` (92), `none` (11)
  - Full ShellCheck compliance (0 errors, 0 warnings)

### Changed

- **Total Audit Scripts**: 253 → 297 (+44)
  - Linux: 118 → 138 (+20)
  - Windows: 123 → 147 (+24)
  - Hypervisors: 12 (unchanged)

### Fixed

- PowerShell null comparison issues (6 files)
- PowerShell automatic variable conflicts ($profile, $event) (5 files)
- Deprecated Get-WmiObject → Get-CimInstance migration (14 files, 23 instances)
- PSScriptAnalyzer: 0 ERROR severity issues remaining

---

## [5.3.0] - 2026-02-20 [UNRELEASED]

### Major Feature Release

Feature-based licensing with activation keys, beta program controls, and comprehensive usage management.

### Added

- **Feature-Based Licensing System** (44 gated features)
  - Remote Sessions: SSH/WinRM interactive terminals require Professional tier
  - Remediation: Fix script execution requires explicit activation keys
  - Integrations: SIEM, ticketing, cloud providers (AWS/Azure/GCP)
  - Advanced Audits: Custom audits, parallel execution, differential analysis
  - Analytics: Trend analysis, predictive analytics, risk scoring
  - Compliance: CIS, HIPAA, PCI-DSS, SOC2, ISO27001 reports
  - Enterprise: Multi-tenant, advanced RBAC, unlimited API/agents

- **Feature Activation Methods**
  - License tier inclusion (Professional/Business/Enterprise)
  - HMAC-signed activation keys (`FEAT-XXXX-XXXX-XXXX-CHECKSUM`)
  - Free trials (7-14 days based on feature risk level)
  - Admin grants with optional time limits

- **Beta/Early Access Licensing**
  - Private Beta: 90-day keys, 100 machines, invite-only
  - Public Beta: 30-day trial, 25 machines, free registration
  - Early Access: 365-day keys, 250 machines, discounted paid tier
  - Internal: Unlimited for development/testing
  - Build expiration (120 days from build date)
  - Usage tracking (daily/monthly scan limits)

- **Feature API Endpoints** (`/api/features/*`)
  - `GET /status` - All features with availability
  - `GET /<feature>/status` - Single feature status
  - `POST /<feature>/activate` - Activate with key
  - `POST /<feature>/trial` - Start free trial
  - `POST /admin/grant` - Admin grant feature
  - `POST /admin/generate-key` - SuperAdmin key generation
  - `POST /admin/bulk-generate` - Bulk key generation

- **Lightweight Agent License Delegation**
  - Core Server allocates license slots to agents
  - Hardware fingerprint tracking for slot assignment
  - Configurable license cache duration
  - Automatic slot release on agent decommission

### Security

- **High-Risk Feature Protection**
  - Fix scripts (Linux/Windows/Hypervisor) require explicit activation keys
  - SSH/WinRM interactive sessions require feature license + SuperAdmin role
  - Session recording requires Business tier
  - Batch fix execution requires Enterprise tier

### Changed

- Terminal routes now check feature licenses before session creation
- Remediation routes require feature key activation, not just license tier
- License delegation config included in deployment packages

### Breaking Changes

- Premium features now require appropriate license tier or activation key
- Fix script execution (`FIX_SCRIPTS_*`) requires explicit key activation
- Interactive terminal sessions require `SSH_INTERACTIVE` or `WINRM_INTERACTIVE` feature

---

## [5.1.5] - 2026-02-17

### Security Patch Release

SSH Key Management, OWASP hardening, and elimination of all MEDIUM severity findings.

### Added

- **SuperAdmin SSH Key Management**
  - Generate ed25519, RSA (2048-4096 bit), and ECDSA keys
  - Download private keys (single-use, never stored on server)
  - Manage authorized keys for SSH authentication
  - Full OWASP A01/A03 compliance with input validation

### Security

- **OWASP Score: 97/100** (A+ rating maintained)
- **Bandit: 0 HIGH, 0 MEDIUM** (all MEDIUM issues resolved)
- SSH key path traversal prevention (blocks `..` in key names)
- SSH key format validation (regex for ed25519/RSA/ECDSA)
- SSH parameter sanitization (blocks shell metacharacters)
- Insecure temp file fix (B108) - UUID suffix for unique paths
- 21 try/except/pass anti-patterns fixed with logging (B110)

### Fixed

- **LDAPBindError redefinition** - Renamed import alias in ldap_auth.py
- **require_api_key redefinition** - Renamed to `_legacy_require_api_key`
- **ScheduledAudit redefinition** - Renamed local class reference
- **Hardcoded password string** - Changed `''` to `None` in password clearing
- **Temp file security** - Added UUID suffix to deploy and discovery temp files

---

## [5.1.4] - 2026-02-16

### Patch Release

Completion of placeholder features, credential management, and 11+ UI/backend improvements.

### Added

- **Credential Update Feature**
  - Edit Server modal now includes credential update section
  - Support for updating password or SSH key authentication
  - SSH key passphrase support for encrypted keys
  - Auth type indicator shows current auth method in modal

- **Admin Panel User Management (Complete)**
  - `editUser()` - Full modal with role and status editing
  - `resetUserPassword()` - Secure password reset with confirmation
  - `deleteUser()` - Confirmation dialog with proper API integration

- **Server Management (Complete)**
  - `editServer()` - Full edit modal with all server properties
  - Credential update section with auth type switching

- **Schedule Management (Complete)**
  - `editSchedule()` - Full modal with plan, frequency, and mode editing
  - GET single schedule endpoint for edit form population

### Fixed

- **CVE Service** - Version range matching now uses semantic version comparison
- **CVE Import** - Offline data import now properly parses and stores CVE records
- **Monitoring Runner** - Audit jobs now actually execute instead of just logging
- **Remediation Dry-Run** - Apply operations now enforce prior successful dry-run within 24 hours
- **GitHub Actions Integration** - Audit re-run trigger now calls repository_dispatch API
- **Ticketing Integration** - Remediation status updates now persist to DifferentialAnalysis records
- **Agent API Validation** - API key validation now checks database settings table
- **Okta Logout** - ID token now stored in session for proper SLO (Single Logout)

---

## [5.1.3] - 2026-02-16

### Patch Release

Enterprise API completion with 12 new endpoints and improved test coverage.

### Added

- **Enterprise API Endpoints**
  - `GET/PUT /api/servers/<id>` - Single server retrieval and update
  - `GET /api/servers?tags=&status=` - Server filtering by tags and status
  - `POST /api/audits` - Create new audit execution
  - `GET /api/audits/<id>` - Get audit execution details
  - `POST /api/audits/<id>/retry` - Retry failed audit
  - `POST /api/audits/<id>/cancel` - Cancel running audit
  - `POST /api/batch/servers/import` - Bulk server import
  - `GET /api/reports/audits/<id>` - Generate audit report
  - `GET /api/reports/audits/<id>/export` - Export as JSON/CSV/PDF
  - `GET /api/analytics/dashboard` - Aggregate metrics dashboard
  - `GET /api/history/servers/<id>/audits` - Server-specific audit history
  - `GET /api/openapi.json` - Machine-readable OpenAPI specification

### Changed

- **Test Coverage** - API integration tests improved from 56% to 93% (42/45 passing)
- **Batch Routes** - URL prefix changed from `/api/v2/batch` to `/api/batch` for consistency

---

## [5.1.2] - 2026-02-16

### Patch Release

Bug fixes, test coverage improvements, and UI enhancements following v5.1.1.

### Added

- **Comprehensive Scheduled Scans Test Suite** - 55 automated tests covering CRUD operations, input validation, authentication, pagination, and OWASP compliance
- **Security Code Analysis Tool** - Static analyzer (`tests/security_code_analysis.py`) for detecting security issues in API code
- **Test & Security Report** - Detailed documentation of test coverage and OWASP compliance (`docs/SCHEDULED-SCANS-TEST-SECURITY-REPORT.md`)

### Fixed

- **Malformed JSON Handling** - Scheduled Scans API now returns 400 (Bad Request) instead of 500 for malformed JSON payloads
- **Settings Page Scheduler Table** - Now displays user-defined scheduled scans instead of internal scheduler jobs
- **Navigation Link** - "Go to Scheduled Scans" shortcut in Settings empty state now works correctly

### Changed

- **Enhanced Logging** - Improved scheduler and app logging for better debugging

---

## [5.1.1] - 2026-02-16

### Patch Release

Bug fixes and improvements following the v5.1.0 release.

### Fixed

- **Navigation URL Fixes** - Asset Discovery navigation shortcuts now correctly link to Reports page
- **Console Navigation** - Fixed Console shortcut linking

### Changed

- **Documentation Updates** - Updated API-GUIDE.md and CONTAINER-DEPLOYMENT.md for v5.1.x

---

## [5.1.0] - 2026-02-15

### Enhanced Asset Discovery Integration

This release significantly improves the Asset Discovery tool integration and adds security hardening.

### Added

- **Enhanced Asset Discovery Scan Modal**
  - 3-step wizard interface (Target → Connection → Options)
  - OS type selection (Linux/Windows/Auto-detect)
  - Discovery method selection: SSH, WinRM, Agent, SNMP
  - Credential configuration with multiple authentication options
  - Test Connection button for connectivity verification
  - Discovery scope options (packages, services, updates, ports, users, security)
  - Schedule options: immediate, once, daily, weekly, monthly
  - Scan summary before submission

- **Skip Schedule Button in Server Wizard**
  - Step 6 (Schedule) now has "Skip Schedule" option
  - Allows manual audit execution without setting up automated schedule

- **Connection Test API Endpoint**
  - New `/api/v1/scans/test-connection` endpoint
  - Real TCP connection testing with timeout handling
  - Hostname resolution verification

### Changed

- **Asset Discovery Docker Integration**
  - Asset Discovery now integrated into main docker-compose.yml
  - Health check dependencies for proper startup order
  - Dual URL configuration (internal container communication + external browser access)

- **Asset Discovery Navigation**
  - Changed from iframe embed to direct redirect
  - Avoids CSP/X-Frame-Options blocking issues
  - Opens in new tab for better user experience

- **ScanJob Model**
  - Added `config` JSON field for enhanced scan configuration
  - Stores OS type, discovery method, credentials, scope, and schedule

### Security

- **Path Traversal Protection** (OWASP score: 97.0/100 A+)
  - Fixed potential path traversal in agent_routes.py
  - Added `sanitize_path()` function to security module
  - Validates hostnames against traversal patterns

- **Hostname Validation**
  - Added `validate_hostname()` function
  - Blocks hostnames containing `..`, `/`, or null bytes
  - Applied to all server-related API endpoints

- **API Rate Limiting**
  - Connection test endpoint limited to 10 requests/minute
  - Scan creation limited to 20 requests/minute

### Fixed

- .dockerignore symlink exclusions for `audits/lib/` and `audits/linux/lib/`
- Docker build warnings for absolute symlinks

---

## [5.0.0] - 2026-02-15

### ⚡ Major Release: Agent-Based Architecture

This release introduces a **fundamental shift** in how the Security Audit Toolkit can be deployed.
In addition to traditional SSH/WinRM-based remote execution, you can now deploy **standalone agents**
that operate autonomously and communicate with the central server via secure HTTPS.

### Three Deployment Methods

| Method | Network Access | Best For |
|--------|---------------|----------|
| **SSH/WinRM** | Requires persistent access | Full control, real-time execution |
| **Lightweight Agent** | Requires SSH/WinRM | Smart recommendations, interactive use |
| **Standalone Agent** | HTTPS only (no SSH/WinRM) | High-security/air-gapped environments |

### Added

- **Standalone Agent for Linux and Windows**
  - Self-contained agent that polls server for commands via HTTPS
  - Can disable SSH/WinRM after deployment for maximum security
  - Daemon mode with configurable poll intervals
  - Automatic audit sync to keep scripts up-to-date
  - Network discovery from inside target networks
  - Push-based result upload (no inbound connections required)

- **Agent Command Queue System**
  - Central server queues commands for agents
  - Agents poll and execute commands autonomously
  - Result upload with automatic retry on failure
  - Command types: audit execution, discovery, sync, configuration

- **Agent Resource Throttling**
  - CPU priority controls (nice/ProcessPriority)
  - Memory threshold checks (pause if low)
  - Network bandwidth limits (upload/download rate limiting)
  - Load average monitoring (Linux) / CPU percentage (Windows)
  - Configurable batch processing with delays

- **Agent Admin Settings** (Web UI)
  - Global throttle configuration in Admin Panel
  - Settings: CPU priority, memory thresholds, network limits
  - Poll/heartbeat intervals configurable
  - Per-agent override capability via API

- **Agent Deployment via Web UI**
  - Deploy standalone agents from the Deploy Panel
  - Configure server URL, API key, schedule during deployment
  - Option to disable SSH/WinRM after agent installation
  - Download agent packages for manual deployment

- **Execution Router**
  - Automatic routing: SSH/WinRM for managed servers, agent queue for standalone
  - Transparent execution regardless of connection method
  - Unified result handling across all deployment types

- **Agent Heartbeat & Monitoring**
  - Agents report health status at configurable intervals
  - Server tracks last-seen timestamps
  - Resource usage reporting (CPU, memory, disk)
  - Throttle configuration sent with heartbeat response

### Changed

- Server model extended with `agent_mode` field (ssh, winrm, agent)
- Deployment workflow updated to support all three modes
- API endpoints now route through execution router
- Documentation restructured around deployment types

### Documentation

- **17-agent-deployment-guide.md** - Complete standalone agent guide
- Updated architecture diagrams for agent-based flow
- Admin settings documentation for throttle configuration
- Troubleshooting guide for agent connectivity issues

### Security

- Agent API uses separate API key authentication
- No inbound connections required for agent mode
- SSH/WinRM can be completely disabled after agent deployment
- All agent-server communication over TLS

### Migration Notes

- Existing SSH/WinRM deployments continue to work unchanged
- New servers can be added in any mode (ssh, winrm, agent)
- Existing servers can be converted to agent mode via UI
- No database migration required (additive changes only)

---

## [4.5.0] - 2026-02-15

### Added

- **Discovery Intelligence Engine** - Smart audit recommendations based on detected software
  - `AUDIT_RECOMMENDATIONS` dictionary with 80+ entries and actual script paths
  - Platform-aware matching for Linux and Windows systems
  - Wizard auto-selects relevant audits based on discovery results
  - API endpoint `/api/discovery/recommendations` returns script paths for direct integration
- **Discovery Performance Options** - Tunable performance for large environments
  - Lightweight mode: Quick scan with reduced overhead (`--mode lightweight`)
  - Throttling: Control discovery speed with millisecond delays (`--throttle 100`)
  - Result limits: Cap results for large fleets (`--limit 1000`)
- **Discovery Script Unification** - Script-first approach with legacy fallback
  - Linux: `linux-discovery.sh` with unified parameters
  - Windows: `windows-discovery.ps1` with matching options
  - MSI deployment includes updated Windows discovery script

### Changed

- Audit Wizard now dynamically populates recommendations from API
- Path-based matching links recommendations to actual audit scripts
- Discovery routes return script paths enabling direct wizard integration
- Linux and Windows recommendations expanded to 40+ entries each

### Architecture

- Clear separation between Audit Tool (recommendations) and Asset Discovery Tool (CVE correlation)
- Asset Discovery Tool remains focused on data collection and vulnerability alerting
- Audit Tool recommendations are web-panel-only, not included in standalone discovery

### Testing

- **Expanded Test Suite**: 329 test files with 3,646 test functions
- Integration tests for discovery recommendations
- Platform-specific test coverage for Linux and Windows audits

## [4.4.0] - 2026-02-15

### Added

- **Developer Script Studio Integration** - Full-featured script development environment integrated into web console
  - Visual script editor with syntax highlighting for Bash, PowerShell, and Python
  - Real-time syntax validation and linting (ShellCheck, PSScriptAnalyzer)
  - Template library with 50+ pre-built audit script templates
  - Compliance mapping tool - auto-map scripts to CIS/NIST/PCI controls
  - Built-in test runner with mock execution environments
  - Script versioning with git integration
  - Export/package scripts for distribution
  - Access via `/script-studio` route in web console
- **Script Studio API Endpoints:**
  - `GET /api/script-studio/templates` - List available templates
  - `POST /api/script-studio/validate` - Validate script syntax
  - `POST /api/script-studio/test` - Test script in sandbox
  - `POST /api/script-studio/save` - Save script to audits directory
  - `GET /api/script-studio/compliance-map` - Get control mappings
- **Platform Support Expanded:**
  - Script Studio supports Linux, Windows, and macOS script development
  - Cross-platform syntax checking and compatibility warnings
  - Auto-detection of OS-specific commands with suggested alternatives

### Changed

- Desktop Electron app deprecated in favor of integrated web-based Script Studio
- Updated deployment bundles to include Script Studio assets
- Enhanced security scan coverage to include Script Studio components

### Security

- Script Studio sandboxed execution prevents system modification
- All scripts validated before execution
- Role-based access: Analyst+ required for Script Studio access
- OWASP security score maintained at **9.5/10 (Excellent)**

### Documentation

- Updated FEATURE-GUIDE.md with Script Studio documentation
- Created RELEASE-PACKAGE-4.4.0.md for marketing release
- Updated deployment guides with Script Studio deployment steps

## [4.3.2] - 2026-02-14

### Added

- **Commercial Documentation Suite** - Complete legal and support documentation for commercial release
  - [EULA.md](EULA.md) - End User License Agreement with BSL 1.1 terms
  - [PRIVACY.md](PRIVACY.md) - Privacy Policy (on-premises, no telemetry)
  - [SUPPORT-POLICY.md](SUPPORT-POLICY.md) - Support expectations for solo-developer model
  - [FAQ.md](FAQ.md) - Frequently asked questions (reduces support burden)
- **Market Analysis Reports** - Release readiness assessment and competitive positioning
  - [MARKET-ANALYSIS-2026-Q1.md](docs/MARKET-ANALYSIS-2026-Q1.md) - Updated market analysis
  - [MARKET-RESEARCH-RELEASE-READINESS-2026.md](docs/MARKET-RESEARCH-RELEASE-READINESS-2026.md) - 92/100 release score
- **Preliminary Pricing Structure** - Competitive launch pricing (subject to change)
  - Community: Free (1-25 servers)
  - Professional: ~$499/year (26-100 servers)
  - Business: ~$1,499/year (101-500 servers)
  - Enterprise: Custom pricing
- **Self-Audit Feature** - Toolkit can now check itself for updates, dependency vulnerabilities, and configuration issues
  - Version checking against GitHub releases
  - Dependency vulnerability scanning via pip-audit
  - Configuration validation (secret key, debug mode, HTTPS, session security)
  - Database migration status checking
  - Git repository health verification
- Self-Audit API endpoints under `/api/superadmin/self-audit/*`
- Self-Audit UI panel in Admin → SuperAdmin Tools section
- CSRF token support added to Reports page (`reports.js`)

### Changed

- **UI Performance** - Reordered page initialization in `admin.js` and `reports.js`
  - Event listeners now bind immediately before async data loading
  - Pages are interactive immediately instead of waiting for API calls
- API Key Management moved to SuperAdmin Tools section (restricted access)
- Removed System Health panel from Admin page (streamlined UI)

### Security

- Added `usedforsecurity=False` to SHA1 hash for HaveIBeenPwned API (fixes Bandit B324 false positive)
- All POST/PUT/DELETE requests in `reports.js` now include CSRF token
- OWASP security score improved to **97.0/100 (A+)**
  - 0 High severity issues (was 1)
  - 10 Medium (all false positives/intentional)
  - 146 Low (informational)

### API Endpoints

- `GET /api/superadmin/self-audit/status` - Quick status check
- `GET /api/superadmin/self-audit/full` - Full diagnostic report
- `GET /api/superadmin/self-audit/version-check` - Check for toolkit updates
- `GET /api/superadmin/self-audit/dependencies` - Scan for vulnerable packages
- `GET /api/superadmin/self-audit/config-check` - Validate security settings

## [4.3.1] - 2026-02-14

### Added

- **Disclaimer HTML Rendering** - Server-side markdown to HTML conversion for styled disclaimer page
- **Compliance Evidence Collector** - Tamper-evident evidence packages for auditors (SHA-256 checksums, ZIP export)
- **Slack/Teams Bot Integration** - Daily security digest, real-time alerts, slash commands
- **ServiceNow/Jira Integration** - Auto-ticket creation for findings, SLA tracking, bi-directional sync
- **GitHub Actions Integration** - Workflow generator, SARIF output, GitHub Security tab integration
- **Trend Analysis APIs** - Compare audit runs, organization-wide improvement tracking
- **HTML Report Enhancements** - Trend visualization, before/after metrics, compliance delta highlighting
- Evidence package export with MANIFEST.json, SHA256SUMS.txt for auditor workflows
- Support for CIS Controls v8, NIST 800-53, PCI-DSS v4.0, ISO 27001:2022, SOC 2, HIPAA
- Comprehensive [AUDIT-SCRIPTS-REFERENCE.md](docs/AUDIT-SCRIPTS-REFERENCE.md) documentation (254 scripts)

### Fixed

- **Disclaimer acceptance form** - Removed JavaScript dependency, using HTML5 native validation
- **Disclaimer link path** - Corrected link from `/static/docs/DISCLAIMER.md` to `/static/DISCLAIMER.md`
- **Server-side markdown rendering** - Replaced CDN-based marked.js with Python `markdown` library

### Security

- All new integration endpoints protected with `@require_auth_and_permission` decorators
- Webhook signature verification via `hmac.compare_digest()` (timing-safe)
- Maintained 96.6/100 (A+) OWASP security score

### API Endpoints

- `POST /api/evidence/collect/execution/<id>` - Collect evidence from audit execution
- `POST /api/evidence/collect/assessment` - Create multi-server evidence package
- `GET /api/evidence/packages/<id>/export` - Export auditor-ready ZIP
- `GET /api/integrations/chat/configs` - List Slack/Teams integrations
- `POST /api/integrations/chat/digest/send` - Send security digest
- `POST /api/integrations/ticketing/tickets/bulk` - Bulk ticket creation
- `POST /api/integrations/github/workflow/generate` - Generate CI/CD workflow YAML

## [4.3.0] - 2026-02-12

### Added

- **Debug Management UI** - Admin panel Diagnostics & Debug section for support team troubleshooting
- Debug API endpoints (`/api/debug/status`, `/api/debug/enable`, `/api/debug/disable`, `/api/debug/client-config`)
- Auto-disable feature prevents debug mode from staying on indefinitely (configurable 5-120 min)
- Comprehensive [DEBUGGING-GUIDE.md](docs/DEBUGGING-GUIDE.md) documentation
- Individual debug toggles for server, client, SQL, SSE, and wizard debugging
- OWASP compliance scorecard at [SECURITY-SCORECARD.md](dev/code-reviews/SECURITY-SCORECARD.md)
- **Kubernetes Deployment Manifests** - Production-ready K8s manifests with HPA, PDB, and Ingress
- Prometheus/Grafana monitoring configuration for Docker stack

### Changed

- Conditional debug logging in JavaScript (WIZARD_DEBUG, SSE_DEBUG flags default to false)
- Updated documentation with correct internal links
- Version references updated to 4.3.0

### Security

- Validated 96.6/100 (A+) OWASP security score
- Debug features require SuperAdmin role
- Debug state persists temporarily and auto-resets on restart

## [4.2.0] - 2026-02-10

### Changed

- **Archived development files** - Moved 70+ files to `archive/` for smaller deployment packages
- Updated 25+ README files with correct `audits/linux/` path structure
- Enhanced documentation with version consistency (4.1.x→4.2.0)
- Deployment packages now exclude archive directory (~10MB savings)

### Added

- `archive/` directory for historical development documentation
- Archive exclusion in `.gitignore` and `.dockerignore`
- Developer Script Studio with license management API
- Password breach checking via HaveIBeenPwned API
- SIEM Integration Guide, Security Overview, Threat Model docs
- Dependabot configuration for automated dependency updates
- Security scanning workflow for GitHub Actions

### Security

- ESLint configuration for JavaScript code quality
- Enhanced input validation with network validators
- Improved schema validation for API endpoints

## [4.1.1] - 2026-02-08

### Security

- Fixed 50+ XSS vulnerabilities in web templates (batch.html, schedules.html, analytics.html)
- Added escapeHtml() sanitization to all JavaScript files
- Hardened Docker Compose for production (required secrets, localhost binding, resource limits)
- Added no-new-privileges security option to containers
- Added log rotation to prevent disk exhaustion

### Added

- .env.example template for secure secret management
- Comprehensive code review reports for all languages
- CODE-REVIEW-STAKEHOLDER-SUMMARY.md executive summary

### Changed

- All API data now escaped before DOM insertion
- Docker ports bound to localhost (127.0.0.1) by default
- Numeric values cast with parseInt() to prevent type coercion attacks

## [4.1.0] - 2026-02-15

### Added

- CVE Database Integration with live vulnerability lookups
- Developer Script Studio for custom audit creation
- HaveIBeenPwned credential breach checking
- Real-time dashboard with 12 widget types
- Differential auditing (compare runs over time)
- SIEM integration documentation (Wazuh, Splunk, ELK, Sentinel)

### Security

- Achieved 96.6/100 (A+) OWASP security score
- Authentication hardening (100/100)
- Session management improvements (100/100)
- Input validation enhancements (95/100)

## [4.0.0] - 2026-02-10

### Added

- **82 Windows audits** covering all major domains
- PowerShell-based audit framework for Windows
- Windows-specific discovery tool
- Active Directory, IIS, SQL Server, Exchange audits
- Group Policy and Windows Defender integration
- Server groups and fleet management

### Changed

- Reorganized audit structure: `audits/linux/` and `audits/windows/`
- API expanded to 200+ endpoints

## [3.0.0] - 2026-02-05

### Added

- RBAC (Role-Based Access Control) with 5 permission levels
- SSO Integration (SAML 2.0 and OIDC providers)
- Multi-tenancy with organization isolation
- PostgreSQL database backend with Alembic migrations
- Docker/Kubernetes production-ready deployment
- Health check endpoints

### Changed

- Complete frontend redesign with dark mode
- WebSocket real-time updates

## [2.0.0] - 2026-02-01

### Added

- 63 Linux audits covering 7 domains
- Discovery tool with 34+ software detection patterns
- NFS/SMB, Terraform, Prometheus/Grafana audits
- Interactive terminal in web UI

### Security

- Command injection prevention
- Path traversal protection
- API authentication & rate limiting
- Session security (HttpOnly, Secure, SameSite)

## [1.0.0] - 2026-01-15

### Added

- Multi-distro compatibility layer (lib/pkg.sh, lib/svc.sh, lib/distro.sh)
- Core platform audits (31 Linux audits)
- Orchestrator v1.0 with basic features
- Web UI v1.0 with Flask backend

### Performance

- Audit discovery caching (98% improvement)
- Regex pre-compilation (10-100x faster)
- SSH connection pooling (80% reduction)
- Parallel execution support

## [0.1.0] - 2025-12-01

### Added

- Initial multi-distro starter, shims, sample audit, and CI.
