# AuditToolkit Linux Security Lite

This repository is the standalone Linux toolkit for expanded host security audits.

Canonical scope definition: see [docs/LINUX-SCOPE-CONTRACT.md](docs/LINUX-SCOPE-CONTRACT.md).

## What this repo is

- Linux-first standalone toolkit
- Bash audit framework with orchestrator and shared compatibility libraries
- Optional lightweight local HTML console via the restored standalone agent
- Enterprise UI pages for compliance, trend analysis, webhook management, and SIEM export (tier-gated)
- CLI workflows for enterprise report/export/trending/webhook operations (same local data model)
- Custom audits are managed by the main platform (Core Server) and consumed by Lite; local custom script authoring is out of scope
- Customer-facing documentation set under `customer-docs/`

## Customer documentation

- Start at `customer-docs/index.md` for lifecycle and quick-start guides
- Folder overview in `customer-docs/README.md`

## Primary runtime paths

- Core audits: audits/linux
- Core libs: lib
- Orchestrator: orchestrator/orchestrator.sh
- Standalone lightweight agent: agents/html-linux
- Linux add-on packaging: addons/linux-audit-agent

## Quick start (WSL/Linux)

```bash
export AUDIT_SKIP_LICENSE_CHECK=1
bash orchestrator/orchestrator.sh --domain linux --dry-run
bash audits/linux/platform/baseline/updates.sh
```

## Lightweight standalone mode

```bash
cd agents/html-linux
./agent.sh status
./agent.sh dashboard
./start-web.sh --daemon

# Enterprise web pages (Professional+/Enterprise)
# http://127.0.0.1:8088/enterprise/compliance
# http://127.0.0.1:8088/enterprise/trending
# http://127.0.0.1:8088/enterprise/webhooks
# http://127.0.0.1:8088/enterprise/siem-export

# Enterprise CLI controls (same host, no browser required)
python3 cli.py compliance --framework cis --days 30 --format json
python3 cli.py siem-export --format cef --severity-filter fail --days 30
python3 cli.py webhooks --action list
python3 cli.py trends --mode scorecard --days 90
```

## Build standalone package

```bash
bash addons/linux-audit-agent/scripts/package.sh
```

## Note on asset-discovery

The full asset-discovery suite under tools/asset-discovery is not the primary product surface for this Linux-Security-Lite repo.
Use the Linux audit toolkit and standalone agent paths above.
