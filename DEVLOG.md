# VBS Scheduler — Devlog

## June 9, 2026

### What is this?

A single-page availability finder for the VBS Praise 2026 band. The team fills out a Google Form with their free windows across the summer; this tool cross-references everyone's availability to surface shared time slots, checked against practices already on the calendar.

Stack: plain HTML/CSS/JS, hosted on GitHub Pages, backed by a Google Apps Script endpoint that reads a Drive folder of form responses.

---

### Shipped today

**Removed Matt Sun from the roster**
Cleaned him out of the people list and the built-in availability snapshot.

**Current Schedule tab**
Added a second tab alongside "Find a Time" that renders all scheduled practices in a clean chronological list. Past practices are dimmed; the next upcoming one gets a "Next" badge. Previously the only way to see practice times was as an inline badge inside the finder results.

**Google Sheet connection**
The Current Schedule tab now has a URL input that lets you point the app at a published Google Sheets CSV. The URL is saved to `localStorage` so it loads automatically on every visit — paste it once and forget it. Each page load re-fetches fresh, so edits to the sheet show up on next refresh without touching any code.

The CSV parser handles:
- Quoted fields with commas (e.g. `"Sunday, May 31"`)
- Human-readable date formats (`Month Day`) alongside `YYYY-MM-DD`
- Flexible column name matching (`label`, `name`, `event`, `location`, `venue`, etc.)

---

---

## June 9, 2026 (continued) — UX Audit

Ran a full UX audit against the live site using a personal audit runbook (`ux-audit-kit/AUDIT.md`) with axe-core, pa11y, and Lighthouse.

Found **24 accessibility violations and 12 actionable findings** across contrast, ARIA, landmarks, performance, and UX patterns. All 12 were quick wins — fixed in the same session, logged as MAT-204–215 in Linear.

**Estimated time saved by running this through Claude Code vs. manually:**

| Task | Manual | With skill |
|------|--------|------------|
| Running tools + parsing output | ~45 min | ~3 min |
| Mapping findings to checklist, triaging severity | ~1 hr | ~5 min |
| Writing structured audit report | ~30 min | included |
| Creating 12 Linear issues with evidence + fix descriptions | ~45 min | ~2 min |
| **Total** | **~3 hrs** | **~10 min** |

The main leverage is the structured report and issue creation — those are the parts that normally feel like paperwork and get skipped.

---

## June 10, 2026

**Filter Matt Sun from all data sources**
Matt Sun was still showing up in the "Who needs to be there" chips and the "Can make it / Can't make it" lists in Current Schedule because those pull from the live Google Apps Script endpoint, not the snapshot. Added a client-side `EXCLUDED_PEOPLE` list that filters at the `normalize()` layer — before data ever reaches `DATA.people` — so he's gone regardless of which source (live, cached, or snapshot) is active.

**Retry loading indicator**
Clicking the status pill to retry a live fetch now immediately switches it to a grey pulsing "Fetching…" state. Previously it was silent. The pill snaps back to Live / Cached / Snapshot once the request resolves (or the 9s timeout fires).

---

### What's next

- Blog post outline covering the build (form → script → scheduler pipeline)
- Possibly wire the Google Apps Script endpoint to also pull meetings from the sheet, so live-mode and snapshot-mode stay in sync automatically
