# Naadbramha — Phase 1: ERP Aesthetics Standardization (Days 1–9)

LogicMotive Consultants Pvt. Ltd. · built into the `naadb` app · 28-Sep-2026

Everything is shipped as app code. Nothing is created by hand on the site, so a
`bench migrate` installs it on the site and a git revert removes it.

## What's included

| Plan day | Deliverable | Where in the app |
|---|---|---|
| 1 | Brand tokens: gold `#F2B81B` + charcoal `#2B2B2B` (placeholders taken from the logo), Poppins headings | `public/css/nb_desk_theme.css` → `:root` |
| 2–3 | Theme for the whole desk (navbar, buttons, sidebar, cards, lists, forms, dark mode) for all companies | `hooks.py` → `app_include_css` |
| 2–3 | Branded hero banner on the NB dashboards | `public/js/nb_desk.js` (imported in `naadb.bundle.js`) |
| 3 | Optional: recolour the existing charts and number cards, with a backup and a restore | `utils/nb_branding.py` |
| 4 | Workspaces: **NB Group Overview**, **NB Management View**, plus one per company (NB Services, NB Food & Beverages, Mauli Distributors, Goingo India) | `naadb/workspace/*`, `naadb/number_card/*`, `naadb/dashboard_chart/*` |
| 5–7 | KPI data model: **NB KRA**, **NB KPI** (manual or auto source), **NB KPI Scorecard** (target vs achievement, weighted score, overall score, band), **NB KPI Settings** | `naadb/doctype/nb_*`, `utils/nb_kpi.py` |
| 8 | **KPI & Performance** page (gauge, KPI progress, leaderboard, bands, department chart, KPIs at risk), KPI workspace, **NB KPI Performance Register** script report | `naadb/page/nb_performance`, `naadb/report/nb_kpi_performance_register` |
| 9 | Weekly pending-achievement reminder (Mon 09:30), plus 50% and 75% milestone alerts to the employee and HOD (in-app and a branded email) | `utils/nb_kpi.py`, `templates/emails/nb_kpi_alert.html`, `hooks.py` → `scheduler_events` |

The existing department dashboards, number cards and their filters are **not modified**.
They are linked from the new workspaces and reused in NB Management View.

## Deploy

1. Commit and push the app, then deploy on Frappe Cloud. This runs `bench build` and `bench migrate`.
2. Hard-refresh the browser (Ctrl+Shift+R) so the desk loads the new CSS and JS.
3. Go to **NB KPI Settings**. **Test Mode is ON by default**, so no employee gets emails.
   Set a Test Recipient, verify the emails, then turn Test Mode off.
4. Optional: load the placeholder KRA and KPI masters
   `bench --site <site> execute naadb.utils.nb_kpi.load_sample_masters`
5. Optional: harmonise the colours of the existing custom charts and cards. A backup file is saved first.
   `bench --site <site> execute naadb.utils.nb_branding.harmonize_colours`
   To undo: `bench --site <site> execute naadb.utils.nb_branding.restore_colours --kwargs "{'file_url': '<backup url printed above>'}"`

## Who sees what (KPI module)

- **HR Manager / HR User / System Manager**: all scorecards. Only these roles can set the KPI, weightage, target and period (permlevel 1).
- **Employee**: their own scorecard. They can enter achievement for manual KPIs.
- **HOD**: the scorecards of their team. The HOD is found through Employee → Reports To → User ID.

## Scoring (placeholder until the client score sheet arrives)

- Achieved % = achievement ÷ target. For "lower is better" KPIs it is target ÷ achievement.
- Weighted score = min(Achieved %, cap 120%) × weightage. Overall score = Σ weighted ÷ Σ weightage.
- Bands: Outstanding ≥ 90, Good ≥ 75, Average ≥ 50, below that Needs Improvement.
- The cap, the bands and the 100% weightage rule are all set in **NB KPI Settings**.

## Pending client inputs

- Brand kit: logo, exact hex codes and fonts. To apply it, only change `:root` in `nb_desk_theme.css`.
- KRA/KPI score sheet, the data source for each KPI, and the HOD mapping. On the test copy, 38 active employees have no *Reports To*,
  and 5 HODs have no user login: Harish Nidavanche, Krishna Nidavanche, Pratiksha Jaware, Payal Gate and Aditya Potphode.
- Production also has an older **KRA Target Assignment** DocType with a "KRA Target Manager" role. Confirm whether it should be migrated into NB KPI Scorecard.
- The plan says 5 companies. The site has 4: NB, NBFB, Mauli Distributors and Goingo.

## Next (Day 10)

100% congratulatory alert, shortfall alert and module testing. Add these to `MILESTONES` and to `send_milestone_alerts` in `utils/nb_kpi.py`.
