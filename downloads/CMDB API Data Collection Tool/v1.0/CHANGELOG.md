# Changelog

All notable changes to the CMDB API Data Collection Tool are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-05-13

### Added

- **Core CMDB Functionality**
  - API-based data collection from multiple sources
  - Configuration Database (CMDB) for centralized asset and configuration management
  - RESTful API for programmatic access to CMDB data
  - Web UI for manual data entry and browsing

- **Managed Agent**
  - Lightweight managed agent for distributed data collection
  - Secure communication with API server
  - Automated inventory collection from remote hosts
  - Support for Windows and Linux environments

- **Integrations**
  - SIEM integration (webhook-based event forwarding)
  - ITSM ticketing system integration
  - SSO/OAuth2 authentication support
  - Custom webhook delivery for third-party systems
  - Service Bus and Event Hub connectors for Azure environments

- **Security and Compliance**
  - Role-based access control (RBAC)
  - API key authentication
  - Enterprise audit logging
  - Security compliance reports (OWASP scorecard alignment)
  - Data encryption for sensitive fields
  - TLS 1.2 and TLS 1.3 support

- **Reporting and Analytics**
  - Vulnerability correlation and CVE tracking
  - Risk scoring and prioritization
  - Customizable compliance reports (PDF export)
  - Dashboard with key metrics and insights
  - Grafana integration for monitoring

- **Data Management**
  - Automated backup and restore functionality
  - Database migrations for version upgrades
  - Support for SQLite and PostgreSQL backends
  - Multi-tenancy support for hosted deployments

- **Administrative Features**
  - Certificate lifecycle management
  - License activation and validation
  - Configuration management via environment variables
  - Comprehensive documentation for deployment and operations
  - Docker containerization for easy deployment

- **Documentation**
  - Complete customer documentation suite
  - Installation and deployment guides
  - Operations and support runbooks
  - API consumer and automation guide
  - Quick-start integration guides
  - Security and compliance documentation

### Security

- Initial security hardening and vulnerability remediation completed
- OWASP security scorecard review and alignment
- Dependency scanning and management
- Secure credential handling
- Protection against common web vulnerabilities

### Known Limitations

- Single-instance deployment recommended for production (clustering not yet supported)
- Managed agent currently supports Windows PowerShell 5.0+ and bash-based Linux systems
- Maximum concurrent API connections: 100 (configurable)

## Release Information

**Release Date:** May 13, 2026  
**Version:** 1.0.0  
**Status:** Stable
