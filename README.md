# Events & Affairs Calendar

A Google Calendar–style web calendar that reads its events live from a Google Sheet. Add or edit an event in the sheet and it shows up on the calendar automatically, with no code changes or redeploys.

**Live calendar:** https://cirhlgn.github.io/directors-calendar/

## Linked Google Sheet

| | |
|---|---|
| **Spreadsheet** | Events and Other Affairs Tracker |
| **Link** | https://docs.google.com/spreadsheets/d/1cGr9LnmuzJ5lw76BkiPhKb1ab5tDfQboH1r76SGF6bU/edit |
| **Spreadsheet ID** | `1cGr9LnmuzJ5lw76BkiPhKb1ab5tDfQboH1r76SGF6bU` |
| **Tab the calendar reads** | `Events and Other Affairs` |

Only the **Events and Other Affairs** tab feeds the web calendar. The other tabs in the spreadsheet (`Calendar`, `TO: maam shanta`) are not read by the page.

## How it works

1. The page fetches the `Events and Other Affairs` tab from the sheet as CSV (via Google's `gviz` endpoint).
2. It re-reads the sheet every **30 seconds**, and again whenever the browser tab becomes active. The ⟳ button refreshes immediately.
3. Each row with an **Event Date** and a **From [Name/Subject]** value becomes an event on that date.

## Adding events

Add a new row in the `Events and Other Affairs` tab. Columns are matched by header name, so reordering columns is safe. Keep the header names unchanged.

| Column | Used for |
|---|---|
| Done | A checked box shows the event struck through |
| Date Received | Shown in the event details |
| Ref. No. | Reference number (e.g. `2026-0001`) |
| **Event Date** | **Required.** The date the event appears on (e.g. `Oct 07, 2026` or `10/7/2026`) |
| **From [Name/Subject]** | **Required.** The event title |
| Action Needed | Shown in the event details |
| Venue | Shown in the event details |
| Status | Controls the colour (see below) |
| Last Activity / Last Reminder / Last Contact | Shown in the event details |

**Times:** put the time in parentheses at the end of the title, for example `HR Orientation (9AM to 12NN)` or `National Shrine of our Lady of the Holy Rosary (4PM)`. The page pulls it out and shows it as the event time. Events without a time show as all-day.

## Status colours

| Status | Colour |
|---|---|
| Urgent | `#891446` |
| Semi-urgent | `#512887` |
| Not urgent | `#062A79` |
| *(blank)* | Plain text, no fill |

Type the status exactly as written above.

## Features

- Month view and Agenda view
- Mini calendar and status filters in the sidebar
- Search box
- Hover for event details; click a day or event for the full card
- Week starts on **Sunday**
- Light and dark mode follow the device setting

## Requirements

The sheet must be shared as **Anyone with the link: Viewer**. If it isn't, the page can't read it and the status indicator in the top bar turns red with a "Can't read sheet" message.

## Settings

Everything configurable is at the top of the `<script>` block in `index.html`:

```js
const CFG = {
  sheetId:   '1cGr9LnmuzJ5lw76BkiPhKb1ab5tDfQboH1r76SGF6bU',
  sheetName: 'Events and Other Affairs',
  refreshSeconds: 30,
  weekStartsOn: 0   // 0 = Sunday first, 1 = Monday first
};
```

To point the calendar at a different sheet, change `sheetId` (and `sheetName` if the tab has a different name), then commit.

## Deployment

This is a single static file (`index.html`) served by GitHub Pages from the `main` branch. To update the calendar's code, edit `index.html` and commit. Changes go live within a minute or two. Changes to events never need a commit, because they are made in the sheet.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Can't read sheet" in the top bar | Set the sheet's sharing to *Anyone with the link: Viewer* |
| An event doesn't appear | Check that the row has both an Event Date and a title, and that the date is a valid date |
| Wrong colour on an event | Check the Status spelling (`Urgent`, `Semi-urgent`, `Not urgent`) |
| Changes not showing | Click ⟳, or hard refresh the page (Ctrl/Cmd + Shift + R) |
