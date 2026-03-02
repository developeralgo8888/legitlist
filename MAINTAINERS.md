# Maintainers Guide

Internal docs for the legitlist core team. Not vendor-facing.

---

## Architecture

When a PR merges into `main`, a GitHub Action syncs all active vendor data to the Framer CMS. The site updates automatically — no manual steps.

```
PR merged → GitHub Action triggers → Vendor data synced to Framer → Site deploys
```

---

## Repo structure

```
legitlist/
├── vendors/
│   ├── _schema.json          # JSON Schema for vendor files
│   ├── _example.json         # Template (active: false, skipped by sync)
│   └── {slug}.json           # One file per vendor
├── logos/
│   └── {slug}.png            # Square, max 200 KB
├── scripts/
│   ├── sync-to-framer.mjs    # Syncs vendors to Framer CMS on merge
│   └── validate-all.mjs      # Local validation script
└── .github/
    ├── workflows/
    │   ├── sync-vendors.yml       # Runs on merge to main
    │   └── validate-vendor.yml    # Runs on every PR
    ├── ISSUE_TEMPLATE/
    │   └── report-vendor.yml      # For reporting a listed vendor
    └── pull_request_template.md
```

Files prefixed with `_` are internal — skipped by sync and validation.

---

## First-time setup

1. Add these secrets under **Settings → Secrets and variables → Actions**:

| Secret | Where to find it |
|---|---|
| `FRAMER_PROJECT_URL` | Framer → Project Settings → API |
| `FRAMER_API_KEY` | Framer → Project Settings → API → Generate Key |

2. Trigger the first sync manually: **Actions → Sync Vendors to Framer CMS → Run workflow**. This creates the Managed Collection in Framer.

---

## Removing a vendor

**Soft remove** — set `"active": false` in the JSON. File stays, vendor disappears from the site.

**Hard remove** — delete the JSON and logo files entirely.

Both trigger the sync on merge.

---

## Local development

```bash
npm install
npm run validate   # validate all vendor files locally
npm run sync       # manual sync (needs .env with FRAMER_PROJECT_URL and FRAMER_API_KEY)
```
