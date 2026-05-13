# CMDB API Data Collection Tool - v1.0.0 Release Documentation

**Release Version:** 1.0.0  
**Release Date:** May 13, 2026  
**Status:** General Availability

---

## Overview

The CMDB API Data Collection Tool is a comprehensive, API-driven platform for collecting, correlating, and reporting on IT asset, configuration, and security data across on-premises environments.

This release introduces v1.0, a stable production-ready version with enterprise-grade features, integrations, and operational support.

---

## What's Included

### Core Components

- **API Server** - Flask-based REST API for CMDB operations
- **Web UI** - Dashboard and administration interface
- **Managed Agent** - Lightweight collection agent for remote systems (Windows/Linux)
- **Database** - SQLAlchemy ORM with SQLite/PostgreSQL backends
- **Connectors** - Pre-built integration modules for SIEM, ITSM, Service Bus, Event Hubs

### Key Features

| Feature | Details |
| --- | --- |
| **Data Collection** | API-based, agent-based, and manual data ingestion |
| **Integrations** | SIEM, ITSM, SSO/OAuth2, webhooks, Azure service connectors |
| **Security** | RBAC, API key auth, audit logging, TLS 1.2+, encrypted secrets |
| **Reporting** | Risk scoring, CVE correlation, compliance reports (PDF), dashboards |
| **Operations** | Automated backup/restore, certificate management, health monitoring |
| **Documentation** | Complete customer docs, ops runbooks, quick-start guides |

---

## System Requirements

### Minimum Recommended

| Component | Requirement |
| --- | --- |
| **OS** | Windows Server 2019+ or Linux (Ubuntu 20.04+, CentOS 8+) |
| **CPU** | 4 cores |
| **Memory** | 8 GB RAM |
| **Storage** | 50 GB (SSD recommended) |
| **Python** | 3.9+ (for source deployments) |
| **Database** | PostgreSQL 12+ (recommended for production) or SQLite |

### Network

- Outbound HTTPS (443) for cloud integrations
- Inbound HTTPS (443) for API and web UI
- Optional: Event Hubs or Service Bus connectivity (Azure integrations)

---

## Installation Methods

### Docker (Recommended)

```bash
docker-compose up -d
```

### Windows MSI Installer

Automated installer available for Windows Server environments.

### Linux Package

RPM and DEB packages available for CentOS/RHEL and Ubuntu/Debian.

### Source Deployment

Manual deployment from source code with Python virtual environment.

---

## What's New in v1.0

This is the first stable release of the CMDB API Data Collection Tool. Major highlights include:

- **Production-ready** stability and performance
- **Comprehensive integrations** with SIEM, ITSM, and cloud platforms
- **Enterprise security** features (RBAC, encryption, audit logging)
- **Managed agent** for distributed data collection
- **Complete documentation** for all roles and use cases
- **Compliance alignment** with ISO/IEC 20000-1:2018 and OWASP standards

See [CHANGELOG.md](CHANGELOG.md) for the complete feature list and changes.

---

## Support

For security vulnerabilities, refer to [SECURITY.md](../../../SECURITY.md).

For other release and support information, see [README.md](README.md).

---

**Last Updated:** May 13, 2026  
**Status:** General Availability (GA)  
**Prepared for:** Release Repository Distribution
