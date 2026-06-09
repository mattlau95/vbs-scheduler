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

### What's next

- Blog post outline covering the build (form → script → scheduler pipeline)
- Possibly wire the Google Apps Script endpoint to also pull meetings from the sheet, so live-mode and snapshot-mode stay in sync automatically
