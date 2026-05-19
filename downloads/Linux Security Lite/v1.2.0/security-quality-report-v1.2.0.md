# Linux Security Lite v1.2.0 Security and Quality Report

Generated: 2026-05-18 09:38:32 +01:00
Release: v1.2.0 (v1.2.0)
Release URL: https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/releases/tag/v1.2.0
Published At: 05/18/2026 07:16:39
Commit: b3b643e8a2277595b8a4042eedc87268d2aafd50

## 1. Executive Summary

AuditToolkit Linux Security Lite v1.2.0 passed release build and safety gates and is assessed as suitable for publication based on available evidence.

Overall security posture: **A+ (OWASP score 10.0/10.0)**.

## 2. Release Build Evidence

- Release workflow run: 26017876097
- Workflow URL: https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/actions/runs/26017876097
- Workflow result: **success**
- Pre-release safety checks: **success**
- Build release packages job: **success**

Validated build steps completed successfully:
- customer-doc release sync validation
- shell script validation in workflow
- package builds for tar.gz, deb, rpm, apk
- artifact upload completion

## 3. Published Release Artifacts

| Artifact | Size | SHA-256 Digest |
|---|---:|---|
| audit-toolkit-lite-1.2.0-r0.apk | 2473.1 KB | sha256:b81d52603916285ca5e2b78a94985f268964d7a7d8da01f2a7990fc70578fdf3 |
| audit-toolkit-lite-1.2.0.noarch.rpm | 2496.4 KB | sha256:b869da2c0fccbfdb9b95816d5fcb2ed8b20be77d16a0fd1b33c28351fd6bee81 |
| audit-toolkit-lite-v1.2.0.tar.gz | 2442.4 KB | sha256:f92b725552e8a87c0747833eec7a56ef21cc23a587e2c51eabb3ec4482da52ae |
| audit-toolkit-lite_1.2.0_all.deb | 2250.4 KB | sha256:98821e2f3bd9934e7e7b348365e079e3a9f6d7a63dddfdf6b514c81f8b4847fe |
| CHANGELOG-v1.2.0.md | 61.8 KB | sha256:7a3123e964f8baaf5389a34485fbefd99b130e3a4908e024a6862259b631264e |
| README-v1.2.0.md | 2.3 KB | sha256:3ab0d5853a1c42287b4a0ac4fc0a3a5e0a20250f1dc5b614974d50a8484f8ea6 |

## 4. Security Validation Results

Primary security evidence (`tools/owasp-score.py --json`):
- OWASP score: **10.0**
- Grade: **A+**
- Python security: Bandit HIGH=0, MEDIUM=0, LOW=0
- Bash/Shell security: ShellCheck errors=0, warnings=0
- Docker checks: pass (non-root user, no plain ADD, no hardcoded ENV secrets, healthcheck present)
- Infrastructure checks: pass (no detected real secrets in `.env`, audit scripts use strict mode, no destructive audit writes)

OWASP Top 10 coverage in score output: A01 through A10 all mapped and covered.

## 5. Quality Validation Results

Release-quality evidence:
- `ci/validate-customer-docs-release-version.py`: **passed for 1.2.0**
- Release Packages workflow: **success** for v1.2.0

Local developer lint note:
- `make lint` produced a formatting delta in `scripts/release-automation.sh` and `update_release.sh` (shfmt style-only output).
- This is a formatting consistency item, not a functional or security defect in release artifacts.

## 6. Residual Risk and Disposition

Residual risk level: **Low**

Rationale:
- No high/medium findings in primary security score evidence.
- Release workflow and safety gates succeeded.
- Remaining local issue is style-format consistency only.

Disposition: **Approved for publication and customer distribution**.

## 7. Verification References

- Release page: https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/releases/tag/v1.2.0
- Workflow run: https://github.com/AuditToolkitLabs/AuditToolkit-Linux-Security-Lite/actions/runs/26017876097
- Local package mirror folders:
  - E:\releases\downloads\Linux Security Lite\v1.2.0
  - C:\Users\churc\Audit Toolkit Labs\Audit Toolkit Customer Release Portal - Documents\Releases\Linux Security Lite\v1.2.0
