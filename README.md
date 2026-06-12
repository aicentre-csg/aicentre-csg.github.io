# AI Centre@CSG

Website of the **AI Centre at City St George's, University of London**.

- **Live site:** https://aicentre-csg.github.io
- **Stack:** [Jekyll](https://jekyllrb.com/) + GitHub Pages, Bootstrap 3, Liquid templating, YAML data files
- **Theme:** based on the [Allan Lab](https://github.com/mpa139/allanlab) template

## Running locally

```bash
bundle install
bundle exec jekyll serve
```

The site is then available at `http://localhost:4000`. The published output in `_site/` is generated and not committed.

## Repository structure

| Path | Purpose |
|---|---|
| `_config.yml` | Jekyll site configuration (title, URL, Markdown settings) |
| `_data/` | All content as YAML — news, publications, events, talks, grants, team, alumni |
| `_pages/` | Site pages (HTML / Markdown with front matter) |
| `_includes/` | Reusable partials — header, footer, news and events sidebars |
| `_layouts/` | Page layouts — `default`, `homelay`, `textlay`, `gridlay`, `team`, etc. |
| `_sass/` / `css/` | Styles — SCSS partials compiled via `css/main.scss` |
| `_plugins/` | Local Jekyll plugins |
| `.github/workflows/` | GitHub Actions for the newsletter and social media automation |
| `.github/scripts/` | Python scripts backing the automation workflows |
| `images/`, `fonts/`, `js/` | Static assets |

## Content sections

### Events, Talks, Grants

Three structured listings, each driven by a YAML data file and rendered by a page:

| Section | Data file | Page |
|---|---|---|
| Events | `_data/events.yml` | `_pages/events.html` |
| Talks | `_data/talks.yml` | `_pages/talks.html` |
| Grants | `_data/grants.yml` | `_pages/grants.html` |

Events and Talks pages split entries into **Upcoming** and **Past**; Grants splits into **Active** and **Completed**. To add content, edit the relevant YAML file — no template changes needed.

### News

`_pages/allnews.html` (the **News** nav link) merges two sources into one chronological feed:

1. Hand-written entries from `_data/news.yml`.
2. Publications from `_data/publist.yml`, synthesised into news items (`New paper: …`).

The page splits into **Upcoming** and **Past** around today's date. `_includes/news.html` is the homepage sidebar variant, showing only recent items.

### Homepage upcoming-events widget

`_includes/events_upcoming.html` shows the next few upcoming events in the homepage sidebar. It renders nothing when there are no upcoming events, avoiding an empty box.

## Templating conventions

- **Date format:** ISO 8601 in all data files. Use `"YYYY-MM-DD"` for a date, or `"YYYY-MM-DDTHH:MM:SS+HH:MM"` when time matters (e.g. `"2026-05-20T14:30:00+01:00"`). Always quoted so YAML keeps the value as a string. Templates render dates as `10 July 2026`, and as `10 July 2026, 14:30` when the value carries a time component.
- **Epoch-sort pattern:** pages that mix and sort dated entries convert each date to epoch seconds, pack the fields as `epoch|||display_date|||headline`, sort the strings (the epoch prefix gives correct chronological order), then split them back for display.
- **Upcoming vs. past:** a 1-day grace buffer (`grace = 86400`) keeps same-day items in the Upcoming section.

## Automation

Two GitHub Actions workflows live in `.github/workflows/`:

### Weekly newsletter draft (`newsletter.yml`)

Runs every Monday at 08:00 UTC (also manually triggerable). It runs `generate_newsletter.py`, which collects upcoming events, recent news, new papers and active grants, and opens a **GitHub Issue** containing the draft for an editor to review and send.

### Social media posting (`social-media.yml`)

Triggers on push when any `_data/*.yml` content file changes. It runs `post_social.py`, which posts new entries to **Mastodon, Bluesky, X (Twitter)** and **LinkedIn**.

### Setup required before automation goes live

1. Add repository secrets (Settings → Secrets and variables → Actions):
   - Mastodon: `MASTODON_INSTANCE_URL`, `MASTODON_ACCESS_TOKEN`
   - Bluesky: `BLUESKY_HANDLE`, `BLUESKY_APP_PASSWORD`
   - X / Twitter: `TWITTER_API_KEY`, `TWITTER_API_SECRET`, `TWITTER_ACCESS_TOKEN`, `TWITTER_ACCESS_TOKEN_SECRET`
   - LinkedIn: `LINKEDIN_ACCESS_TOKEN`, `LINKEDIN_PERSON_URN`
2. Set repository variable `SOCIAL_DRY_RUN=true` to log posts without publishing while testing.
3. Test the newsletter manually: Actions → "Weekly Newsletter Draft" → "Run workflow".

> Note: `social-media.yml` currently triggers on pushes to the `main` branch — confirm this matches the branch GitHub Pages actually publishes from before relying on it.

## Adding content

1. Edit the relevant file in `_data/` (see the tables above).
2. Commit and push to the publish branch — GitHub Pages rebuilds automatically.
3. If automation secrets are configured, a social media post fires for new `_data/` entries.

## Admin editor

An admin editor has been added in `admin/` for live content updates.
It uses a username/password login and GitHub API commits to update the same `_data/` files.
Deploy the admin app to Vercel and set the required environment variables.

See [WORKLOG.md](WORKLOG.md) for the development history.
