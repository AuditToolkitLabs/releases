# Changelog

All notable changes to this project will be documented in this file.

The format is based on Keep a Changelog, and this project follows Semantic Versioning.

## [Unreleased]

### Added

- Added customer-facing security assurance report for planned version
  1.1.0 at
  `customer-docs/asset-command-center/14-customer-security-assurance-report.md`.

### Documentation

- Updated planned release version references to 1.1.0 across customer
  EULA and customer-facing README/appendix documentation.
- Updated security reports to include release-scope Bandit evidence and
  transparent `--ignore-nosec` validation reporting.

## [1.0.2] - 2026-05-11

### Changed

- Release packaging now includes README.md and CHANGELOG.md in
  GitHub release assets.
- Windows MSI payload staging now includes CHANGELOG.md alongside
  existing metadata files.
- Release validation now checks that README.md and CHANGELOG.md are
  present in MSI harvested payload metadata.
