# Changelog

<!-- markdownlint-disable MD013 -->
<!-- Line length rule disabled - changelog entries benefit from single-line readability -->

All notable changes to this project will be documented in this file.

## [1.1.5] - 2026-05-10 (Audit Coverage and WSL Validation Release)

### Added

- New Linux security audits: added dedicated audits for RabbitMQ, Kafka, Consul, Logstash, and Prometheus under `audits/linux/apps/` and `audits/linux/platform/monitoring/`.
- WSL validation tooling: added `ci/wsl-testhost.sh` for single-distro execution and `ci/wsl-matrix.ps1` / `ci/wsl-matrix.sh` for multi-distro matrix runs.

### Changed

- Customer documentation release sync: updated customer-facing docs to release `v1.1.5`, refreshed metadata dates, and aligned index/release references.
- Shell formatting normalization: applied repository-wide shell formatting alignment required by lint policy (`shfmt -i 2 -ci`).

### Validated

- Cross-distro test coverage: validated toolkit execution across WSL distributions (Ubuntu, Debian, openSUSE, Fedora, AlmaLinux, OracleLinux, Arch).
- Quality gates: lint gates passing for ShellCheck, shfmt, and Flake8.

## [1.1.4] - 2026-05-05 (Release Sync and LFS Cleanup)

### Fixed

- Updater reliability under strict bash mode: corrected `compare_versions` call handling in `scripts/toolkit-updater.sh` so semantic non-zero returns are captured safely under `set -euo pipefail` instead of aborting update/patch flows.
- Phase 7 version-gate smoke pathing: `ci/tests/test-upgrade-preserves-state.sh` now writes the smoke target version into the staging path used by updater gate logic.

### Added

- Upgrade lifecycle hardening controls: added downgrade/re-install gating defaults and post-upgrade wrapper health verification with automatic rollback guardrails.
- Install/rollback safety improvements: partial-install cleanup trap, `SKIP_BACKUP` sentinel behavior, and root checks before package-manager operations for `.deb`/`.rpm` flows.

### Changed

- Git LFS decommissioning for release flow: removed repository LFS tracking rules as release artifacts are now produced via GitHub Actions release jobs.
- Customer documentation release sync: updated customer docs and release references to `v1.1.4`.
