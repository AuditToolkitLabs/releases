# releases

Public distribution hub for AuditToolkit Labs product release assets. Source code is
maintained in private repositories and is never published here.

## Purpose

This repository exists to host **GitHub Release assets**. It is a distribution endpoint,
not a working repository:

- Product binaries, installers, checksums, signatures, SBOMs and release notes are
  attached to GitHub Releases here.
- The git tree itself holds only this README and the validation workflow. No product
  source, no binaries in-tree.
- The public website links directly to
  `https://github.com/AuditToolkitLabs/releases/releases/download/<tag>/<asset>`.

## How releases get here

Publishing is **not** performed by hand in this repository, and not by any workflow in
it. It is driven from the private site repository (`audit-toolkit-preview`):

```text
Gitea (private, local)  →  ci/sync-releases.py  →  GitHub Releases on this repo  →  website
```

`ci/sync-releases.py` reads `releases/releases-sources.json` (which names this repo as
`github_repo`), then for each product runs `gh release create` with the release notes and
`gh release upload` for the assets.

The sync is **manual** — it runs when invoked, not on a schedule.

To publish or re-publish, work in `audit-toolkit-preview`, not here.

## Tag naming

Releases use per-product namespaced tags so that several products can coexist in one
repository:

```text
audit-toolkit-v1.1.4
grithian-v2.3.2
kithian-v1.1.6
borian-v1.1.4
```

All tags currently point at the same commit on `main`; the tag is an addressing label for
the release and its assets, not a snapshot of code.

> Note: an earlier revision of this README described a "strict no-tag policy" and a
> ruleset blocking tag creation. That policy was retired — namespaced tags are now how
> assets are addressed, and the website depends on them. Do not re-enable a tag-creation
> restriction without first migrating the site's download URLs.

## Source protection

[`.github/workflows/validate-release-assets.yml`](.github/workflows/validate-release-assets.yml)
runs on `release: published` and is the gate against accidental source disclosure. It:

1. Polls the API until every asset on the release has settled to `state: uploaded`
   (assets are uploaded *after* the release is created, and large ones take minutes, so
   the webhook payload cannot be trusted for this).
2. Rejects assets whose names look source-like, and unpacks any archive to reject
   source-like paths inside it.

GitHub also auto-generates `Source code (zip)` / `(tar.gz)` for every tag. Because tags
here point at this repository's own tree — which contains no product source — those
archives are harmless. Keep it that way: never commit product source, build scripts or
customer-specific material to this repository.

## File requirements for uploaded assets

- Allowed: binaries, installers, checksums, signatures, SBOMs, provenance, release notes.
- Not allowed: source code, build scripts, project files, source archives.

## Retired mechanisms

The following were removed and should not be reintroduced:

- **SharePoint distribution.** Releases were once catalogued in a `downloads/` tree whose
  `external-links.json` files pointed at an `audittoolkitlabs.sharepoint.com` customer
  portal. SharePoint is no longer used for releases; those links are stale and invalid.
- **`publish-downloads-site.yml`** — built a GitHub Pages index from that `downloads/`
  tree. The Pages site was never successfully built and nothing linked to it.
- **`mirror-audit-tool-release.yml`** — a weekly cron that mirrored assets from the old
  single-repo `AuditToolkitLabs/Audit-Tool-`. Superseded by `ci/sync-releases.py`.
