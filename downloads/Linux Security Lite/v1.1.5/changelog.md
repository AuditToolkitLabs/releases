# Changelog

<!-- markdownlint-disable MD024 -->
<!-- Duplicate heading names (Added, Changed, Fixed) are expected in changelog version sections -->

All notable changes to this project will be documented in this file.

## [1.1.5] - 2026-05-10 (Audit Coverage and WSL Validation Release)

### Added

- **New Linux security audits:** added dedicated audits for RabbitMQ, Kafka, Consul, Logstash, and Prometheus under `audits/linux/apps/` and `audits/linux/platform/monitoring/`.
- **WSL validation tooling:** added `ci/wsl-testhost.sh` for single-distro execution and `ci/wsl-matrix.ps1` / `ci/wsl-matrix.sh` for multi-distro matrix runs.

### Changed

- **Customer documentation release sync:** updated customer-facing docs to release `v1.1.5`, refreshed metadata dates, and aligned index/release references.
- **Shell formatting normalization:** applied repository-wide shell formatting alignment required by lint policy (`shfmt -i 2 -ci`).

### Validated

- **Cross-distro test coverage:** validated toolkit execution across WSL distributions (Ubuntu, Debian, openSUSE, Fedora, AlmaLinux, OracleLinux, Arch).
- **Quality gates:** lint gates passing for ShellCheck, shfmt, and Flake8.

## [1.1.4] - 2026-05-05 (Release Sync and LFS Cleanup)

### Fixed

- **Updater reliability under strict bash mode:** corrected `compare_versions` call handling in `scripts/toolkit-updater.sh` so semantic non-zero returns are captured safely under `set -euo pipefail` instead of aborting update/patch flows.
- **Phase 7 version-gate smoke pathing:** `ci/tests/test-upgrade-preserves-state.sh` now writes the smoke target version into the staging path used by updater gate logic.

### Added

- **Upgrade lifecycle hardening controls:** added downgrade/re-install gating defaults and post-upgrade wrapper health verification with rollback guardrails.
- **Install/rollback safety improvements:** partial-install cleanup trap, `SKIP_BACKUP` sentinel behavior, and root checks before package-manager operations for `.deb` / `.rpm` flows.

### Changed

- **Git LFS decommissioning for release flow:** removed repository LFS tracking rules as release artifacts are now produced via GitHub Actions release jobs.
- **Customer documentation release sync:** updated customer docs and release references to `v1.1.4`.

## [1.1.3] - 2026-05-05 (Upgrade Safety Hardening Release)

### Fixed

- Corrected package identity handling in `scripts/toolkit-updater.sh` from `audit-tool` to `audit-toolkit-lite`.
- Corrected repository slug references to `AuditToolkit-Linux-Security-Lite`.
- Added `scripts/toolkit-updater.sh` to package manifest include list.
- Replaced destructive update copy flow with safer sync behavior and fallback logic.
- Guarded DEB `prerm` wrapper removal to avoid breaking upgrade paths.
- Updated reinstall behavior in `install.sh` to prefer `git pull --ff-only` where possible.

### Added

- CI regression test `ci/tests/test-upgrade-preserves-state.sh` covering upgrade safety invariants.
- Pre-release safety gate in `.github/workflows/release-packages.yml`.

### Changed

- `ci/run-phase7-tests.sh` now includes Upgrade Preserves State as a registered suite.

### Upgrade Notes

No schema changes and no breaking changes to output format. `scripts/toolkit-updater.sh` is now included in release packages (tarball, `.deb`, `.rpm`).

## [1.1.1] - 2026-05-03 (Signed Security Documentation Release)

### Added

- OWASP security scorecard at `docs/OWASP-SECURITY-SCORECARD.md`.
- Customer-facing copy at `customer-docs/OWASP-SECURITY-SCORECARD.md`.

### Changed

- OWASP scorecard updated to `100/100 (A+)` and A08 marked fully closed for signing controls.
- `SECURITY.md` updated to align with current posture.

### Fixed

- Markdown lint violations and local artifact ignore patterns.

## [1.0.1] - 2026-05-01

### Fixed

- ShellCheck issues (SC2009, SC2154, SC2016, SC1091) across key Linux audit scripts and CI bootstrap paths.

### Changed

- OWASP scorecard updated with May 2026 revalidation addendum.

## [1.0.0] - 2026-04-30 (Purpose Alignment Release)

### Added

- Purpose-aligned implementation roadmap completion across phases 1-8.
- Documentation and CI purpose-validation improvements for stable schema and audit completeness reporting.

### Changed

- Schema v1.0 marked production-stable for rollout.

### Known Limitations

- NixOS and dinit init systems are not yet supported.
- Alpine Linux coverage remains reduced due to minimal base image constraints.
