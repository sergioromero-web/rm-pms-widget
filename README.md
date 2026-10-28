# RM PMS · Zoho CRM widget

Rental Management PMS shown as a Web Tab inside Zoho CRM. It reads and writes
RM Tenancies, RM Fee Lines and Installments with the permissions of the user
who opens it. No keys or servers.

## Structure

```
plugin-manifest.json   Zoho widget manifest (Web Tab)
app/widget.html        The whole app: UI, logic and Zoho JS SDK calls
.github/workflows/     Builds the uploadable zip on every push and release
```

## Release a new version

1. Change `app/widget.html`, commit and push to `main`.
2. Tag the version: `git tag v0.1.1 && git push --tags`.
3. GitHub Actions validates and packs the widget and attaches the zip to the release.
4. Zoho CRM: Setup → Developer Hub → Widgets → RM PMS → Edit → upload the new zip → Save.

## Local check (optional)

```
npm install -g zoho-extension-toolkit
zet validate
zet pack        # creates dist/<name>.zip
```

## Data used

| Module | API name | Use |
| --- | --- | --- |
| RM Tenancies | RM_Tenancies | Tenancies, stages, checks, payment totals |
| RM Fee Lines | RM_Fee_Lines | Monthly fees, deduction checks, Finance list |
| Installments | Installments | Tenant payins and landlord payouts (read only) |
| Accounts | Accounts | Landlord VAT number and folder link |

Users need read access to these modules and write access to RM Tenancies and RM Fee Lines.
