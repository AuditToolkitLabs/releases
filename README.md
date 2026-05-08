# releases
Public release artefacts for AuditToolkit Labs products. Source code is maintained in private repositories.

## Purpose

This repository is the public distribution point for product release assets.

- Source code remains in private repositories.
- Built binaries, installers, and checksums are published here as GitHub Release assets.
- Release tags follow the pattern `<product>/<version>`.

## Public Access Model

- The repository is public so anyone can download published release assets.
- Private source repositories are not exposed through this repository.
- Do not upload source bundles unless they are intentionally public.

## Release Requirements

Every published GitHub Release must include at least one uploaded asset.

The CI workflow in [`.github/workflows/validate-release-assets.yml`](.github/workflows/validate-release-assets.yml) enforces this rule on `release.published` events and fails if a release has zero assets.

## Recommended Asset Set

For each release, upload:

- Platform binaries/installers
- SHA256 checksum file
- Optional signature or provenance file

## Verification

To verify current coverage, check release asset counts via GitHub API:

```powershell
$headers = @{"User-Agent"="audit-toolkit-release-check"}
$uri = "https://api.github.com/repos/AuditToolkitLabs/releases/releases?per_page=100"
(Invoke-RestMethod -Uri $uri -Headers $headers) |
  Select-Object tag_name, @{Name='asset_count';Expression={$_.assets.Count}} |
  Format-Table -AutoSize
```
