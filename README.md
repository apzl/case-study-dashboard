# Heidi lead dashboard (static)

Two static pages, no build step and no server code. The data comes from the lead pipeline in its
own repository: https://github.com/apzl/case-study

- `index.html` – the lead list: overview, Berlin map, filters, table, per-lead card with evidence.
- `pipeline.html` – "How it works": stages, sources, checks, scores and costs (aggregates only).
- `data.js` – the data for `index.html` (`window.HEIDI_DATA = {...}`), **redacted** for publishing.

The pages load the Geist font from Google Fonts; everything else is inline. They also work from
`file://`.

## Hosting

Any static host works; upload this folder as is.

- **GitHub Pages:** Settings → Pages → deploy from the default branch, folder `/ (root)`.
- **Netlify:** drag and drop this repository folder on app.netlify.com/drop.
- **Locally:** open `index.html`, or `python -m http.server 8000`.

The "Code on GitHub" link in each page header is a single anchor with `id="repoLink"`; change its
`href` there (one place per page).

## Refreshing the data

Clone the pipeline repository next to this one (`../case-study`), then from the pipeline repository
root, after `python -m heidi_leads.pipeline export`:

```bash
python -m heidi_leads.dashboard_data            # full data -> output/dashboard/data.js (local, git-ignored)
python -m heidi_leads.dashboard_data --public   # redacted copy -> ../case-study-dashboard/data.js
```

Use `--dashboard-dir PATH` (or `HEIDI_DASHBOARD_DIR`) if this repository lives elsewhere. The full
build also copies these pages into the pipeline's `output/dashboard/`, so the complete version with
named contacts can be opened locally there; never commit that file here.

## Privacy

The published `data.js` keeps companies, tiers, scores, the business phone line, shared inboxes
(info@, kontakt@, office@ ...), website, district, map position, facts and their quotes. It removes
the contact person, role, direct phone and e-mail, LinkedIn profiles, the list of named contacts,
contact evidence, any e-mail address that is not a generic inbox, and quotes that name a person
(those facts stay, with their source page but without the quote). Counts and statistics are kept,
so the overview still shows, for example, how many leads have a named contact. The lead card says
"Named contact withheld" where the full version shows a person.

Never publish `output/dashboard/data.js`: it carries named people, personal phone numbers, e-mails
and LinkedIn profiles.
