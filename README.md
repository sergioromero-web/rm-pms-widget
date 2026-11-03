# RM PMS · Zoho CRM widget

Rental Management PMS shown as a Web Tab inside Zoho CRM (tab **RM PMS**). It reads and
writes Zoho CRM data with the permissions of the user who opens it. No keys or servers.

Current version: **v0.6.0**

## Structure

| Path | What it is |
| --- | --- |
| plugin-manifest.json | Zoho widget manifest (Web Tab) |
| app/widget.html | The whole app: UI, logic and Zoho JS SDK calls |
| .github/workflows/ | Builds the uploadable zip on every push and release |

## Views

| View | What it is for |
| --- | --- |
| **Overview** | Daily dashboard: 5 key figures, the month at a glance, Today (what needs someone, by deadline), Next 30 days, Tenancies by stage and a collapsed Data to fix |
| **Tenancies** | List with vetting, key gate, overdue rent and data-gap dots; upcoming vacancies and deposits |
| **Tenancy page** | Stage bar, Next step with one-click actions, checks grouped by key gate, money, people and follow-up, property rules, month-by-month payments |
| **Landlords** | One card per landlord: active tenancies, rent, next payout, fee next month, pending actions |
| **Properties** | Next available date per property with a six-month timeline. From Spotahome bookings; manual Stargate blocks are not included yet |
| **Finance** | Fee list for Finance (Manual Invoicing sheet columns), landlord statements that reconcile, collections, fee revenue and deductions to review |

## Rules shared with the CRM functions

- **Key gate**: contract signed + deposit received + first rent paid + vetting Accepted (no vetting needed without a booking). Same rule as rm_daily_stages_and_tasks.
- **Automatic stages** every morning: Contract signed, Ready for move-in, Active, Ending, Closed. Changed by the team: Contract sent, Moved out, Cancelled.
- **Statements**: our fee only counts when it was really deducted, so rent - fee - VAT - other = paid. Same rule as rm_generate_statement.
- **Fees for Finance**: amount without VAT and with 21% Spanish VAT; landlord account Id from the booking.
- **Test records**: landlords whose name contains "test" are hidden unless Show test records is ticked.

## Data used

| Module | API name | Use |
| --- | --- | --- |
| RM Tenancies | RM_Tenancies | Tenancies, stages, checks, vetting, payment totals |
| RM Fee Lines | RM_Fee_Lines | Monthly fees, real deductions (Retool), Finance list |
| RM Properties | RM_Properties | Property rules, keys, insurance, preferences, availability (SG_ fields) |
| Installments | Installments | Tenant payins and landlord payouts (read only) |
| Booking Requests | Deals | Address, plan, first payment, vetting, contacts (read only) |
| Accounts | Accounts | Landlord VAT number, contact and folders |
| Tasks | Tasks | Open tasks created by rm_daily_stages_and_tasks |
| Case Management | Case_Management | Complaints of the booking (read only) |

## Related CRM functions

| Function | When | What it does |
| --- | --- | --- |
| rm_sync_tenancy_payments | Daily 07:00 | Payment totals and monthly fee lines from Installments |
| rm_sg_sync_all | Daily 07:00 | Next available dates from Spotahome bookings |
| rm_daily_stages_and_tasks | Daily 07:15 | Stages, key gate, vetting and tasks |
| rm_confirm_fees_from_payments | 24th, 07:30 | Fee really deducted, from the payments database via Retool |
| rm_fee_list_to_finance | 25th, 09:00 | Fee list and deduction confirmation to Finance |
| rm_monthly_statements | 6th, 09:00 | Landlord statements (PDF) when the month reconciles |
| rm_generate_statement | On demand | One landlord statement (Generate button) |

## Release a new version

1. Copy the new widget.html into app/ and check it is the right one.
2. Commit and push to main.
3. Tag the version (git tag v0.6.1, then git push --tags). GitHub Actions packs the widget and attaches the zip to the release.
4. Zoho CRM: Setup > Developer Hub > Widgets > RM PMS > Edit > upload the new zip > Save, then Ctrl + F5.

## Changelog

### v0.6.0 · 3 Nov 2026
- Overview rebuilt as a dashboard; finance blocks moved to Finance, vacancies and deposits to Tenancies.
- New Properties view with a six-month availability timeline from Spotahome bookings.
- Data gaps by severity: money-blocking gaps in Today, collapsed summary on the overview, dots in the tenancy list.
- Tenancy page: Next step with one-click actions, detections, property panel, people and follow-up.
- Vetting is part of the key gate.
- Statements only count the fee that was really deducted.
- Calm visual system; test records hidden by default; "Sync payments now" removed.

### v0.3 · 29 Oct 2026
- Overview, Tenancies, Landlords and Finance views; live money figures; landlord statements with Generate.
