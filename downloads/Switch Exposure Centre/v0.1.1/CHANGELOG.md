# Changelog

All notable changes to Switch Exposure Center will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.1] - 2026-05-19

### Fixed

- **WSL Connectivity Issue**: Fixed Flask API bind address configuration to support WSL2 network forwarding to Windows host
  - Changed hardcoded `127.0.0.1` bind to configurable `0.0.0.0` default in systemd API unit
  - Added `SWITCH_EXPOSURE_BIND_HOST` environment variable (defaults to `0.0.0.0`)
  - Updated systemd service to reference bind host via environment variable
  - This resolves Windows browser access failures when running API service in WSL

### Changed

- Deploy systemd unit now uses configurable bind host via `${SWITCH_EXPOSURE_BIND_HOST}` variable
- Environment example template includes `SWITCH_EXPOSURE_BIND_HOST=0.0.0.0` configuration

### Testing & Quality

- ✓ All 210 unit tests passing (100%)
- ✓ Systemd deployment test updated to verify configurable bind host
- ✓ Bandit static analysis completed
- ✓ No new security issues introduced

## [0.1.0] - 2026-05-12

### Added

- Initial production release for Ubuntu Server LTS deployment
- Multi-vendor switch inventory collection (Cisco, Juniper, Arista, Aruba, Dell, Brocade, Cisco MDS)
- Advisory and CVE correlation by firmware, OS train, and platform
- CVSS, KEV, and remediation-oriented reporting
- Flask API service with health and readiness endpoints
- SQLAlchemy ORM with SQLite and PostgreSQL support
- Static web console for operational visibility
- Connector workers for collection and normalization
- Advisory enrichment pipeline for vendor bulletins
- LDAP authentication and OIDC integration support
- Runtime licensing with Keygen provider
- Comprehensive backup and restore capabilities
- External scheduler support for advisory automation
- nginx reverse proxy with self-signed TLS termination
- Systemd-managed API, worker, and scheduler services
- Debian package (.deb) for automated Ubuntu Server LTS deployment
- Release artifact integrity verification via SHA-256 manifest

### Security

- Stack trace exposure sanitization in error responses
- Security headers in nginx reverse proxy (CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy)
- LDAP connectivity error details sanitized from API responses
- Licensing error responses sanitized
- Entitlement matrix enforcement for identity endpoints
- OWASP grade A security rating

### Quality Assurance

- 205/205 unit tests passing
- 0 CodeQL security alerts (all alerts remediated)
- 0 Bandit findings across 8,805 lines of code
- Full test coverage across advisory, connector, API, and service layers
- Production-ready deployment checklist

### Documentation

- Deployment guide for Ubuntu Server LTS single-host deployment
- Linux runbook with service management and health check guidance
- API contracts and payload specifications
- Data model documentation
- Advisory feed validation guide
- Backup and restore runbook with drill procedures
- Customer security and quality assurance report
- Enterprise implementation and operations guides

### Fixed

- nginx `http2` directive compatibility for Ubuntu 24.04 (removed standalone directive, compatible with nginx 1.24+)
- Licensing error responses no longer expose raw exception details
- LDAP connectivity test responses no longer expose socket or bind errors

### Infrastructure

- Packaged systemd service units for API, worker, and scheduler
- nginx configuration with baseline security headers
- Release manifest generation and verification tooling
- Package post-installation automation for environment setup

## Release Notes

### Installation

1. Download the Debian package from the [GitHub release page](https://github.com/AuditToolkitLabs/Switch-Exposure-Center/releases/tag/v0.1.0)
2. Verify package integrity using the provided SHA-256 manifest
3. Install with: `sudo dpkg -i switch-exposure-center_0.1.0_all.deb`
4. Confirm services are active: `systemctl status switch-exposure-api switch-exposure-worker switch-exposure-scheduler`

### Supported Platform

- Ubuntu Server LTS (20.04, 22.04, 24.04)

### Known Limitations

- Single-host deployment only in this release (multi-host and containerized deployments planned for future releases)
- SAN-specific fields for Brocade and Cisco MDS use `custom_attributes` until real customer payloads drive schema promotion
- Private vendor advisory coverage requires customer-supplied credentials or export files

### Next Steps

- Multi-host deployment support
- Containerized deployment (Docker/Kubernetes)
- Enhanced NETCONF and SNMP fallback transports
- Expanded vendor coverage based on customer requirements

---

**For detailed release information, see:**

- [Deployment Guide](docs/DEPLOYMENT.md)
- [Linux Runbook](docs/LINUX-RUNBOOK.md)
- [Security and Quality Assurance Report](docs/customer-docs/switch-exposure-center/26-security-and-quality-assurance-report.md)
