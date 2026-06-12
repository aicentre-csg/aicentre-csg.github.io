# Work Log

Development history of the AI Centre@CSG website.

## Main development steps so far

1. **Site setup** — stood up a Jekyll site on GitHub Pages from the Allan
   Lab theme and configured it for the organisation repository.
2. **Launch event** — built the AI Centre launch event page and tuned the
   homepage (carousel, group description).
3. **Structured content sections** — added Events, Talks and Grants, each
   driven by a YAML data file and an upcoming/past (or active/completed)
   page, plus nav links.
4. **Unified news feed** — merged hand-written news with publications
   synthesised from `publist.yml`, and added a homepage upcoming-events
   widget.
5. **Automation** — added GitHub Actions for a weekly newsletter draft
   (as a GitHub Issue) and for posting new content to social media.
6. **Fixes and polish** — corrected the news page sort order and tidied
   navigation labels.

Detailed entries below, newest first.

## 2026-06-12 — ISO 8601 date migration

- All `_data/*.yml` dates now use ISO 8601 (`YYYY-MM-DD`, or
  `YYYY-MM-DDTHH:MM:SS+HH:MM` when a time is needed).
  - `news.yml`, `events.yml`, `talks.yml`: converted from
    `"D Month, YYYY"`.
  - `grants.yml`, `publist.yml`: `YYYY/MM/DD` → `YYYY-MM-DD`.
- Templates now parse dates directly with `| date: '%s'`; the
  `date_to_xmlschema` round-trip is gone.
- Display format unified to `%-d %B %Y` (e.g. `10 July 2026`). When a
  data value contains `T` it is rendered with time appended
  (`10 July 2026, 14:30`).
- Files touched: `_pages/events.html`, `_pages/talks.html`,
  `_pages/allnews.html`, `_includes/news.html`,
  `_includes/events_upcoming.html`.

## 2026-06-12 — Events / Talks sort order fix

- `_pages/events.html` and `_pages/talks.html` sorted descending, so the
  Upcoming section listed the farthest-future entry first instead of the
  next one. Now sort ascending for Upcoming and reuse `sorted_reverse`
  for Past — same pattern as `_pages/allnews.html`.

## 2026-05-22 — News page fixes

- Fixed a Liquid bug in `_pages/allnews.html`: the **Past** section used
  `{% for entry in sorted | reverse %}`, but Liquid cannot apply a filter
  inside a `for` tag, so entries were iterating in unreversed order. Now
  `sorted_reverse` is assigned first.
- Shortened the **News** nav label (was "News & Events") and the page's
  section headings to "Upcoming" / "Past".
- Added `@charset "UTF-8";` to `css/main.scss`.
- Commit: `d4f0365`.

## 2026-05-02 — Events / Talks / Grants sections and automation

Added structured listings and automation to the site. Committed as
`619b660`; open as PR #1 on the `feature/events-grants-talks-automation`
branch.

**New content sections**

- Data files: `_data/events.yml`, `_data/talks.yml`, `_data/grants.yml`.
- Pages: `_pages/events.html`, `_pages/talks.html`, `_pages/grants.html`
  — Events and Talks split into Upcoming / Past, Grants into Active /
  Completed.
- Nav links for Events, Talks and Grants in `_includes/header.html`.

**News feed**

- `_pages/allnews.html` and `_includes/news.html` synthesise publications
  from `_data/publist.yml` into news items alongside `_data/news.yml`
  entries.
- Homepage sidebar widget `_includes/events_upcoming.html` shows the next
  few upcoming events; wired into `_layouts/homelay.html`.

**Automation**

- `.github/workflows/newsletter.yml` + `.github/scripts/generate_newsletter.py`
  — weekly GitHub Issue with a newsletter draft (Mondays 08:00 UTC).
- `.github/workflows/social-media.yml` + `.github/scripts/post_social.py`
  — posts new `_data/` entries to Mastodon, Bluesky, X and LinkedIn.

**Notes**

- An earlier attempt branched this work against the wrong upstream repo
  (`mpa139/allanlab`, the theme template). That was unwound: stray forks
  and PR removed, and the work re-applied cleanly to this repository.
- Automation is not yet live — see the setup checklist in
  [README.md](README.md#setup-required-before-automation-goes-live).

## 2026-01-06 — Content update

- Updated `_data/research_fellow.yml`.

## 2025-12 — AI Centre launch event

- Added and iterated on the launch event page
  (`_pages/event-opening-2025.md`) and its images.
- Updated launch event details in `_data/news.yml`.
- Adjusted the homepage carousel in `index.md` (disabled / commented out
  unused items) and revised the group description.

## 2025-11-24 — Initial site setup

- Initial Jekyll site based on the Allan Lab theme.
- Configured for the main organisation repository (`aicentre-csg.github.io`).
- Initial publications page.
