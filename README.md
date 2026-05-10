# releases

Public release artefacts for AuditToolkit Labs products. Source code is maintained in private repositories.

## Purpose

This repository is the public distribution point for product release assets.

- Source code remains in private repositories.
- Built binaries, installers, and checksums are published through a static downloads catalog.
- No tags are created in this repository.

## Public Access Model

- The repository is public so anyone can download published release assets.
- Private source repositories are not exposed through this repository.
- Do not upload source bundles.

## Binary Distribution Method

Public downloads are published from the `downloads/<tool>/v<version>/` folders and deployed as a GitHub Pages site by the workflow in [`.github/workflows/publish-downloads-site.yml`](.github/workflows/publish-downloads-site.yml).

Each tool gets its own top-level folder, and each release version gets its own subfolder.

## Source Protection Controls

The workflow in [`.github/workflows/validate-release-assets.yml`](.github/workflows/validate-release-assets.yml) blocks source-like uploaded Release assets.

Even with that protection, GitHub generates source archives for tags.

To prevent source archive exposure, this repository follows a strict no-tag policy.

## Tag Ruleset Checklist (GitHub UI)

Create a repository ruleset in GitHub to block tag creation.

1. Open repository settings, then `Rules`, then `Rulesets`.
2. Create a new ruleset and target `Tags`.
3. Set tag name pattern to `*`.
4. Enable rule: `Restrict creations`.
5. Enable rule: `Restrict updates`.
6. Enable rule: `Restrict deletions`.
7. Keep bypass list empty, or limit bypass to one emergency admin role only.
8. Turn on `Do not allow bypassing the above settings` if your policy allows it.
9. Save and enable the ruleset.

After enabling, verify by attempting to create a test tag from a non-admin account.

## Publishing Steps

1. Copy binaries, checksums, and signatures into the correct `downloads/<tool>/v<version>/` folder.
2. Commit and push to `main`.
3. Wait for the Pages deployment workflow to finish.
4. Validate downloads from the published Pages URL.

For example:

- `downloads/audit-toolkit/v6.4.4/`
- `downloads/audit-fleet-agent/v6.4.4/`
- `downloads/release/v6.4.4/`

## File Requirements

- Include only binaries, installers, checksums, signatures, and provenance files.
- Exclude source code, build scripts, project files, and source archives.

## Tag and Source Archive Caveat

GitHub automatically exposes source archives (`Source code (zip)` and `Source code (tar.gz)`) for each tag in this repository.

If this repository is public and tags exist, those generated source archives are available even when uploaded assets are binaries only.

To prevent that, use one of these approaches:

- keep this repository private
- avoid creating public tags here and publish binaries from a distribution-only repository
