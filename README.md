# AI Centre@CSG

Website of the **AI Centre at City St George's, University of London**.

- **Live site:** https://aicentre-csg.github.io
- **Stack:** [Jekyll](https://jekyllrb.com/) + GitHub Pages, Bootstrap 3, Liquid templating, YAML data files
- **Theme:** based on the [Allan Lab](https://github.com/mpa139/allanlab) template

## The below is not yet fully implemented/set up

### Things to decide
- local user management vs Github (or other)?
- social media not implemented/wired up 

## How to run this locally

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

## Admin Editor Workflow

Updated: 2026-06-19

The admin editor now lives in the `admin/` submodule, which points to:

- Admin app repo: `https://github.com/SyedHuq28/ai-csg-admin.git`
- Current admin app commit recorded by this site repo: `fa57f1d`
- Admin Vercel app: `https://ai-csg.vercel.app`

The admin app is a small Next.js app deployed separately from the GitHub Pages site. It updates the website by committing to YAML files in the configured GitHub repo, opening or updating one shared pull request, and then allowing a reviewer to publish or reject it.

### Roles and URLs

| Role | URL | Purpose |
|---|---|---|
| Content admin | `https://ai-csg.vercel.app/dashboard` | Add content, preview the submitted entry, then Confirm or Cancel |
| PR reviewer | `https://ai-csg.vercel.app/review` | Review confirmed changes, preview them, approve and publish, reject, or open GitHub |

There are two separate login pairs:

- Dashboard login: `ADMIN_USERNAME` and `ADMIN_PASSWORD`
- Review login: `ADMIN_PR_USERNAME` and `ADMIN_PR_PASSWORD`

Do not commit the actual values. Set them in Vercel environment variables.

### Required Admin Environment Variables

Set these on the Vercel project for `ai-csg`:

| Variable | Purpose |
|---|---|
| `ADMIN_USERNAME`, `ADMIN_PASSWORD` | Login for content admins |
| `ADMIN_TOKEN` | Server-side token for dashboard API calls |
| `ADMIN_PR_USERNAME`, `ADMIN_PR_PASSWORD` | Login for reviewers |
| `ADMIN_PR_TOKEN` | Server-side token for review API calls |
| `GITHUB_TOKEN` | GitHub token used by the admin APIs |
| `GITHUB_WRITE_REPO` | Repo where admin edits are committed |
| `GITHUB_TARGET_REPO` | Repo where the pull request is opened |
| `GITHUB_PREVIEW_REPO` | Optional repo for Vercel deployment statuses |
| `GITHUB_BRANCH` | Base branch for PRs, usually `main` |
| `GITHUB_DRAFT_BRANCH` | Draft branch for queued edits, default `admin-drafts` |
| `VERCEL_PREVIEW_URL_TEMPLATE` | Optional fallback external preview URL |

The GitHub token needs:

- `contents:write`
- `pull_requests:write`

### Current Content Flow

1. A content admin logs in at `/dashboard`.
2. The admin selects a section: News, Events, Talks, Grants, or Publications.
3. The admin fills the form and clicks Save.
4. The admin API appends the entry to the matching YAML file:
   - News: `_data/news.yml`
   - Events: `_data/events.yml`
   - Talks: `_data/talks.yml`
   - Grants: `_data/grants.yml`
   - Publications: `_data/publist.yml`
5. The edit is committed to the configured draft branch, normally `admin-drafts`.
6. The app opens one shared PR if none exists, or updates the existing open admin PR.
7. The submitted form data is stored as hidden PR metadata so the admin app can preview it without relying on an external Vercel iframe.
8. The dashboard Pending changes panel shows:
   - `Open preview`
   - `Confirm`
   - `Cancel`
9. `Open preview` opens `/preview?view=dashboard`, showing the submitted entry as simple organised text.
10. `Confirm` marks the PR as ready for review. It disappears from `/dashboard` and appears on `/review`.
11. `Cancel` closes the PR and deletes the draft branch.
12. A reviewer logs in at `/review`.
13. The reviewer sees the confirmed PR and can:
   - open the same simple preview at `/preview?view=review`
   - approve and publish
   - reject
   - view the PR on GitHub
14. `Approve & publish` squash-merges the PR and deletes the draft branch.
15. GitHub Pages rebuilds the live site from `main`.

### Preview Behaviour

As of 2026-06-19, the admin preview is deliberately simple.

It does not embed the Vercel site preview because the Vercel preview domain can refuse iframe connections. Instead, the admin preview page shows the submitted fields in an organised format that matches the website content structure closely enough for review.

For new pending changes, the preview shows full field details. Older PRs created before this change may only show summary lines.

### Deployment Notes

The admin app must be pushed to `SyedHuq28/ai-csg-admin.git`. Vercel should redeploy `ai-csg` automatically from that repo.

This site repo tracks the admin app as a submodule. When the admin app is updated, also update and push the submodule pointer in this repo.

Latest workflow commits on 2026-06-19:

- Admin repo: `fa57f1d Render admin previews without iframe`
- Site repo: `7e68efe Update admin preview workflow`

### Optional Site Preview Project

A separate Vercel site preview project can still exist for rendered Jekyll previews:

- Framework: Other
- Install command: `bundle install`
- Build command: `bundle exec jekyll build`
- Output directory: `_site`

This is now optional for the admin workflow. The dashboard/review preview no longer depends on that external preview URL.

See [WORKLOG.md](WORKLOG.md) for the earlier development history.
