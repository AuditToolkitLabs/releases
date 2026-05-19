# Release Setup Guide for AuditToolkit Labs Releases Repository

This guide explains how to set up a new product release in the `releases` repository so it displays correctly on the GitHub Pages downloads site.

## Repository Structure

The releases repository uses GitHub Pages to generate an HTML downloads catalog from a simple folder structure:

```text
releases/
└── downloads/
    └── <Product Name>/
        └── v<version>/
            ├── README.md
            ├── CHANGELOG.md
            ├── external-links.json
            └── (other supporting documents)
```

## The Setup Process

### Step 1: Create the Release Folder

Create a new folder under `downloads/` for your product version:

```text
downloads/Your Product Name/v1.0.0/
```

Folder names are case-sensitive and will appear exactly as named on the site.

### Step 2: Create external-links.json

This file maps all assets to their download locations (typically SharePoint). It determines what appears on the release page.

**Location:** `downloads/Your Product Name/v1.0.0/external-links.json`

**Format:**

```json
[
  {
    "name": "package-name.deb",
    "url": "https://audittoolkitlabs.sharepoint.com/:f:/s/AuditToolkitCustomerReleasePortal/YOUR_SHAREPOINT_LINK",
    "size": "",
    "badge": "SharePoint"
  },
  {
    "name": "CHANGELOG.md",
    "url": "https://audittoolkitlabs.sharepoint.com/:f:/s/AuditToolkitCustomerReleasePortal/YOUR_SHAREPOINT_LINK",
    "size": "",
    "badge": "SharePoint"
  },
  {
    "name": "README.md",
    "url": "https://audittoolkitlabs.sharepoint.com/:f:/s/AuditToolkitCustomerReleasePortal/YOUR_SHAREPOINT_LINK",
    "size": "",
    "badge": "SharePoint"
  }
]
```

**Key Points:**

- List packages first, then documentation files
- All entries should point to the same SharePoint folder
- Set `"size": ""` (empty) - the site generator will display "SharePoint" as the source instead
- Badge should always be `"SharePoint"` for consistency

### Step 3: Create README.md

This file contains product documentation and will display inline on the release page under the "README" section.

**Location:** `downloads/Your Product Name/v1.0.0/README.md`

**Content:** Use your standard product README. Example structure:

```markdown
# Product Name

Product description and overview.

## What this product is

- Key feature 1
- Key feature 2

## Quick start

Instructions for getting started.

## Documentation

Links to detailed docs.
```

**Note:** This README will be rendered as HTML inline, so use standard Markdown formatting.

### Step 4: Create CHANGELOG.md

This file documents version changes and will display inline under the "Release Notes" section.

**Location:** `downloads/Your Product Name/v1.0.0/CHANGELOG.md`

**Format:** Use [Keep a Changelog](https://keepachangelog.com/) format:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

## [1.0.0] - 2026-05-19

### Added
- New feature description

### Changed
- Modified feature description

### Fixed
- Bug fix description
```

### Step 5: Add Supporting Documents (Optional)

Any other markdown files in the folder (beyond README.md and CHANGELOG.md) can be listed in `external-links.json` and will appear on the release page as downloadable assets.

Examples:

- Security assessment reports
- License/EULA documents
- Security gate documentation
- Test/audit reports

### Step 6: Commit and Push

```bash
cd releases
git add downloads/Your\ Product\ Name/v1.0.0/
git commit -m "Add Your Product Name v1.0.0 release"
git push origin main
```

The GitHub Actions workflow (`.github/workflows/publish-downloads-site.yml`) will automatically:
1. Generate the HTML pages
2. Create the product card on the main page
3. Create the release page with all assets and inline documentation
4. Deploy to GitHub Pages

## Site Generation Details

### How Files Are Rendered

The site generator processes each release folder:

1. **Asset List** - From `external-links.json`
   - Displays as numbered list with name, link, and badge
   - All assets point to SharePoint
2. **Release Notes Section** - From `CHANGELOG.md`
   - Inline markdown rendering
   - Converted to HTML with basic formatting (headings, bold, code, lists)
3. **README Section** - From `README.md`
   - Inline markdown rendering
   - Same formatting as Release Notes

### File Exclusions

Files that appear in `external-links.json` are:

- **Excluded** from the local file listing
- **Included** in the asset list instead
- Linked to their SharePoint URL

This prevents duplication and ensures all files point to SharePoint, not the GitHub repository.

## Example: Complete Release Setup

For a product `Security Toolkit v2.0.0`:

```text
downloads/
└── Security Toolkit/
    └── v2.0.0/
        ├── README.md                          ← Product documentation
        ├── CHANGELOG.md                       ← Version history
        ├── external-links.json                ← Asset mapping
        └── (no binary files here)
```

**external-links.json:**

```json
[
  {
    "name": "security-toolkit_2.0.0_all.deb",
    "url": "https://audittoolkitlabs.sharepoint.com/:f:/s/AuditToolkitCustomerReleasePortal/YOUR_FOLDER_LINK",
    "size": "",
    "badge": "SharePoint"
  },
  {
    "name": "security-toolkit-2.0.0.rpm",
    "url": "https://audittoolkitlabs.sharepoint.com/:f:/s/AuditToolkitCustomerReleasePortal/YOUR_FOLDER_LINK",
    "size": "",
    "badge": "SharePoint"
  },
  {
    "name": "CHANGELOG.md",
    "url": "https://audittoolkitlabs.sharepoint.com/:f:/s/AuditToolkitCustomerReleasePortal/YOUR_FOLDER_LINK",
    "size": "",
    "badge": "SharePoint"
  },
  {
    "name": "README.md",
    "url": "https://audittoolkitlabs.sharepoint.com/:f:/s/AuditToolkitCustomerReleasePortal/YOUR_FOLDER_LINK",
    "size": "",
    "badge": "SharePoint"
  }
]
```

Result on GitHub Pages:

- **Main page:** Card showing "Security Toolkit" with version count
- **Product page:** Link to v2.0.0
- **Release page:** 2 assets listed + CHANGELOG + README inline

## Troubleshooting

### Release isn't showing on the site

1. **Check folder name:** Must be under `downloads/` and contain a `v` prefixed version folder (e.g., `v1.0.0`)
2. **Check for README.md and CHANGELOG.md:** Both must exist for inline rendering
3. **Check external-links.json:** Must be valid JSON; use a JSON validator
4. **Wait for workflow:** GitHub Pages deployment takes 30-60 seconds after push

### Assets aren't showing

1. **Verify external-links.json entries:** Check that all asset names match their intended files
2. **Check SharePoint URL:** Ensure the URL is valid and accessible
3. **Verify JSON syntax:** Invalid JSON will cause the entire file to be ignored

### README/CHANGELOG not rendering

1. **Check file naming:** Must be exactly `README.md` and `CHANGELOG.md` (case-sensitive)
2. **Verify Markdown syntax:** Invalid Markdown may not render; validate with a Markdown linter
3. **Check for comments:** Remove HTML comments (`<!-- comment -->`) if they cause parsing issues

## Quick Reference

| File | Required? | Purpose | Displayed As |
| --- | --- | --- | --- |
| `external-links.json` | Yes | Asset mapping | Asset list with links |
| `README.md` | Yes | Product docs | Inline section |
| `CHANGELOG.md` | Yes | Version history | Inline section |
| Other `.md` files | No | Supporting docs | Asset list if in external-links.json |

## Repository Workflow

1. **Push to main branch** → GitHub Actions triggered
2. **Workflow reads `downloads/` folder structure** → Generates HTML
3. **Deploy to GitHub Pages** → Available at `audittoolkitlabs.github.io/releases/`
4. **Each product → product page** at `audittoolkitlabs.github.io/releases/Product%20Name/`
5. **Each version → release page** at `audittoolkitlabs.github.io/releases/Product%20Name/vX.Y.Z/`

---

That's it! With this simple folder structure, the GitHub Pages generator handles the rest.
