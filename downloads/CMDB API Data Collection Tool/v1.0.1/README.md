# CMDB API Data Collection Tool

**Current Release:** [v1.0.1](customer-docs/CHANGELOG.md#101---2026-05-15) (May 15, 2026)  
**Status:** General Availability (Patch Release)

API-based CMDB data collection and reporting tool built with Flask, SQLAlchemy,
and a lightweight managed agent.

## Included Inline Documentation

- [RELEASE-v1.0.1.md](RELEASE-v1.0.1.md): Full customer release notes for v1.0.1.
- [CHANGELOG.md](CHANGELOG.md): Version-by-version change history.
- [36-release-deployable-security-gate.md](36-release-deployable-security-gate.md): Release-scope security gate and OWASP release note.
- [LICENSE-EULA.md](LICENSE-EULA.md): Licence terms and permitted-use obligations.

## SharePoint Release Folder

- [Release assets folder](https://audittoolkitlabs.sharepoint.com/:f:/s/AuditToolkitCustomerReleasePortal/IgBbPUcfpK_qQJxU0n01GunBAQpAGOt2v7am73tUoYlxw5E?e=USfV4u)

## Licence and Permitted Use

This project is licensed under the **Business Source License 1.1 (BSL 1.1)**.

### Permitted Use (Summary)

The Software may be used **only** for:

- non-production evaluation, testing, and internal development; and
- internal IT asset, configuration, and security data collection
  within environments controlled by the Licensee.

### Prohibited Use (Summary)

Without a separate commercial agreement, the Software **must not** be:

- used in production environments;
- offered as a hosted service, managed service, or SaaS;
- embedded in or distributed with another product;
- used for resale, OEM, or other revenue-generating activity; or
- operated for the benefit of third parties (including managed customers).

Commercial production use requires a separate licence agreement with the Licensor.

### Authoritative Terms

This section is a **summary only**. The full and binding terms are defined in:

- `LICENSE-EULA.md`
- `LICENSE` (Business Source License 1.1)

If there is any conflict, those documents take precedence.

## Security and Responsible Use

This Software handles **sensitive operational and security-related data**,
including host inventory, configuration details, software versions,
and vulnerability indicators.

Users are expected to operate the Software responsibly and securely.

In particular, you should:

- restrict access to authorised administrators only;
- secure API endpoints, credentials, tokens, and licence keys;
- avoid exposing the application to the public Internet without
  authentication, network controls, and monitoring;
- apply security updates in a timely manner; and
- ensure collected data is handled in accordance with your
  organisation's information security policies.

Improper or insecure deployment may:

- materially increase risk to your environment; and
- constitute a breach of the licence terms.

See `LICENSE-EULA.md` §11A for formal security obligations.

## Which File Should I Download?

Use the release asset that matches your role and platform:

- Core server on Ubuntu or Debian: `cmdb-tool_<ver>_amd64.deb`
- Core server on RHEL or Rocky Linux: `cmdb-tool_<ver>.x86_64.rpm`
- Core server on Windows: `cmdb-tool-windows-server-installer-x64-<ver>.msi`
- Core server in-place update on Linux: `cmdb-tool-update-linux-<ver>.tar.gz`
- Core server in-place update on Windows: `cmdb-tool-update-windows-<ver>.zip`
- Windows agent with guided installer: `cmdb-agent-windows-installer-x64-<ver>.msi`
- Windows agent for manual or portable deployment: `cmdb-agent-windows-portable-x64-<ver>.zip`
- Linux agent bundle: `cmdb-agent-linux-x64-<ver>.tar.gz`
- macOS agent bundle: `cmdb-agent-macos-x64-<ver>.tar.gz` or `cmdb-agent-macos-arm64-<ver>.tar.gz`
- BSD agent bundle: `cmdb-agent-freebsd-x64-<ver>.tar.gz` or `cmdb-agent-openbsd-x64-<ver>.tar.gz`
- Container deployment of the core server: GHCR image published with each release

Quick guidance:

- Choose a `cmdb-tool` package when you are deploying the central CMDB application server.
- Choose a `cmdb-tool-update` bundle when you are patching an existing core server in place and want to preserve deployed database, configuration, and collected data.
- Choose a `cmdb-agent` package when you are deploying a monitored endpoint agent.
- Choose an `installer` file when you want a guided OS-native setup experience.
- Choose a full installer or OS package for first install, platform migration, or when your normal software-distribution tooling expects native packages.
- Choose a `portable` zip or tarball when you want manual deployment or custom packaging.

From `v1.0.0` onward, every core-server release also ships targeted update bundles plus `apply_targeted_update.py`. The updater creates a pre-change backup, refuses to overwrite protected data paths, and then runs database migrations separately after patching the application files.

Quick in-place update examples for an existing core-server deployment (upgrade from v1.0.0 to v1.0.1):

Linux:

```bash
python3 apply_targeted_update.py \
  --bundle cmdb-tool-update-linux-1.0.1.tar.gz \
  --install-root /opt/cmdb-tool \
  --config-path /etc/cmdb-tool/cmdb-tool.conf
```

Windows PowerShell:

```powershell
python .\apply_targeted_update.py `
  --bundle .\cmdb-tool-update-windows-1.0.1.zip `
  --install-root "C:\Program Files\CmdbTool" `
  --config-path "C:\ProgramData\CmdbTool\cmdb-tool.env"
```

Use `--dry-run` first if you want to preview backup creation, patched files, and the migration command without changing the installation.

> **Commercial / MSP / OEM use**
>
> If you intend to:
>
> - run this software in production,
> - provide it as a service to third parties,
> - manage customer environments, or
> - embed it in another product
>
> you **must** contact the Licensor to obtain an appropriate commercial licence.
>
> **AuditToolkitLabs** — Michael Churchill trading as AuditToolkitLabs
> 4th Floor, Silverstream House, 45 Fitzroy Street, Fitzrovia, London W1T 6EB
> Tel: +44 (0) 20 8090 9610
> Sales & Licensing: [License@audittoolkitlabs.com](mailto:License@audittoolkitlabs.com)
> General enquiries: [admin@audittoolkitlabs.com](mailto:admin@audittoolkitlabs.com)

## Licence Enforcement and Telemetry

The Software includes optional licence-validation and usage reporting
capabilities.

- Telemetry is **disabled by default**
- When enabled, telemetry reports only high-level, non-content metadata
  (e.g. licence tier, number of workspaces, number of endpoints,
  software version)
- No collected inventory data, host data, vulnerability data, secrets,
  or personal data are transmitted

Disabling telemetry does **not** remove the obligation to comply with
licence terms.

Deliberate attempts to bypass licence enforcement mechanisms or
falsify usage information may result in termination of licence rights.

## Features

- Flask backend with SQLAlchemy models:
  - `Host`
  - `CollectedData`
  - `CustomAPI`
  - `Agent`
- REST API endpoints for host management, custom API management, agent
  management, and data collection
- Data collectors:
  - Direct API collector
  - Custom API collector
  - Agent-based collector
- Web UI:
  - Dashboard
  - Host detail and collection controls
  - Custom API configuration
  - Agent management
- Managed agent server that exposes system info over HTTP
- Pytest test suite for API endpoints and collectors

## Project Structure

```text
CMDB-API-DATA-COLLECTION-TOOL/
├── app/
│   ├── __init__.py
│   ├── api.py
│   ├── collectors.py
│   ├── extensions.py
│   ├── models.py
│   ├── web.py
│   ├── static/
│   │   └── style.css
│   └── templates/
│       ├── agents.html
│       ├── api_config.html
│       ├── base.html
│       ├── dashboard.html
│       └── host_detail.html
├── managed_agent/
│   └── agent_server.py
├── tests/
│   ├── conftest.py
│   ├── test_api.py
│   └── test_collectors.py
├── requirements.txt
└── run.py
```

## Prerequisites

- Python 3.10+
- `pip`

## Setup

For production or sensitive environments, review the licence and
security obligations defined in `LICENSE-EULA.md` before use.

### Installation

1. Create and activate a virtual environment.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

1. Install dependencies.

```powershell
pip install -r requirements.txt
```

1. Start the Flask app.

```powershell
python run.py
```

## Cross-Platform Universal App Strategy

This repo also includes a cross-platform scaffold for producing no-install
artifacts on Windows and Linux.

### Goals

- Universal Windows support
- Universal Linux support
- No runtime installs
- No distro-specific builds
- Single-file executables where possible

### Included Assets

- Workspace file: `crossplatform.code-workspace`
- Build scripts:
  - `build/windows/build.ps1`
  - `build/linux/build.sh`
- Scaffold folders:
  - `src/app/`
  - `src/platform/`
  - `src/shared/`
- Strategy notes: `docs/workspace-notes.md`

### Build Commands

Windows:

```powershell
./build/windows/build.ps1
```

Linux:

```bash
./build/linux/build.sh
```

### Platform Recommendations

Windows:

- .NET self-contained: best for GUI and enterprise integration
- Go static binary: best for portable CLI/agent tooling

Linux:

- Go static binary: best universal distro portability
- .NET self-contained: strong cross-platform option for APIs/services

The app runs at `http://127.0.0.1:5000`.

## Database

- Default database: `sqlite:///cmdb.db`
- Tables are auto-created at app startup.
- Optional CLI command:

```powershell
flask --app run.py init-db
```

## REST API

Base path: `/api`

### Hosts

- `GET /api/hosts`
- `POST /api/hosts`
- `GET /api/hosts/<host_id>`
- `PUT /api/hosts/<host_id>`
- `DELETE /api/hosts/<host_id>`
- `POST /api/hosts/<host_id>/collect`
- `GET /api/hosts/<host_id>/data`

Create host example:

```bash
curl -X POST http://127.0.0.1:5000/api/hosts \
  -H "Content-Type: application/json" \
  -d '{"name":"app-host","base_url":"http://localhost:5001/info","description":"primary node"}'
```

### Custom APIs

- `GET /api/custom-apis`
- `POST /api/custom-apis`
- `GET /api/custom-apis/<api_id>`
- `PUT /api/custom-apis/<api_id>`
- `DELETE /api/custom-apis/<api_id>`

Create custom API example:

```bash
curl -X POST http://127.0.0.1:5000/api/custom-apis \
  -H "Content-Type: application/json" \
  -d '{"name":"metrics","endpoint":"{host_base_url}/metrics","method":"GET"}'
```

### Agents

- `GET /api/agents`
- `POST /api/agents`
- `GET /api/agents/<agent_id>`
- `PUT /api/agents/<agent_id>`
- `DELETE /api/agents/<agent_id>`

Create agent example:

```bash
curl -X POST http://127.0.0.1:5000/api/agents \
  -H "Content-Type: application/json" \
  -d '{"name":"local-agent","host":"127.0.0.1","port":9000,"enabled":true}'
```

## Data Collection Modes

### 1. Direct API Collector

Collects data by sending `GET` request to `Host.base_url`.

```bash
curl -X POST http://127.0.0.1:5000/api/hosts/1/collect
```

### 2. Custom API Collector

Uses a configured `CustomAPI` object.

```bash
curl -X POST http://127.0.0.1:5000/api/hosts/1/collect \
  -H "Content-Type: application/json" \
  -d '{"type":"custom","custom_api_id":1,"payload":{"scope":"all"}}'
```

### 3. Agent-based Collector

Collects from registered managed agent endpoint `/system-info`.

```bash
curl -X POST http://127.0.0.1:5000/api/hosts/1/collect \
  -H "Content-Type: application/json" \
  -d '{"type":"agent","agent_id":1}'
```

## Web UI

Open `http://127.0.0.1:5000` and use:

- Dashboard (`/`): create hosts, view summary, browse latest collected data
- Host Detail (`/hosts/<id>`): run direct/custom/agent collection and inspect
  records
- API Config (`/apis`): create and list custom APIs
- Agent Management (`/agents`): register agents and toggle enabled state

## Managed Agent

Run the lightweight managed agent:

```powershell
python managed_agent/agent_server.py --host 0.0.0.0 --port 9000
```

Agent endpoint:

- `GET /system-info`

Example response fields include hostname, OS, Python version, CPU count, IP,
and timestamp.

## Testing

Run all tests:

```powershell
pytest -q
```

Test coverage includes:

- API CRUD tests for hosts, custom APIs, and agents
- Collector tests for direct, custom, and agent-based modes
- Collection endpoint tests for all three collection methods

## Notes

- This implementation uses SQLite by default for simplicity.
- For production usage, move to a managed database and secure API endpoints
  with authentication and authorization.
