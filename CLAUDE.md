# Bucket List project

A self-contained, offline-capable visualization of an experiential bucket list,
organized by the best month of the year to do each experience.

## Where the data lives

**All data lives in the `DATA` array inside [`bucket-list.html`](bucket-list.html).**
There is no database, build step, or external file. Open `bucket-list.html` in a
browser to view it.

## Data model: place → events

`DATA` is an array of **places**. Each place groups one or more **events**
(distinct experiences you can have at that place, often in different seasons).

```js
const DATA = [
  {
    place: "Yosemite National Park",   // place name
    region: "California, USA",         // where it is
    events: [
      {
        title: "Firefall at Horsetail Fall", // subheading (place is the main heading)
        months: [2],                          // best month(s): array of 1-12, OR "all"
        category: "nature",                   // nature | city
        scope: "trip",                        // "trip" (multi-day) | "daytrip" (Bay Area drive)
        duration: "1-2 days",                 // how long it takes
        highlights: [                         // optional sub-bullets: what to look for
          "Horsetail Fall glows orange before sunset"
        ],
        note: "Mid-to-late February only.",   // optional extra detail
        prereq: "Reserve parking ahead."      // optional advance prep / permit
      }
    ]
  }
];
```

### Field meanings

| Field      | Where    | Meaning |
|------------|----------|---------|
| `place`    | place    | Name of the place. |
| `region`   | place    | Country / state / area. |
| `events`   | place    | Array of experiences at that place. |
| `title`    | event    | The specific experience. Shown as the **subheading** on the card — the `place` name is the main heading, so one place can list several titled events. |
| `months`   | event    | Array of month numbers `1`–`12` for the optimal month(s), **or** the string `"all"` for genuinely year-round experiences. |
| `category` | event    | One of `nature`, `city`. Drives color and the category filter toggles. |
| `scope`    | event    | One of `trip` (multi-day trip that needs planning) or `daytrip` (single excursion drivable from the Bay Area). Drives the **Trip type** filter toggles. |
| `duration` | event    | Free-text length, e.g. `"2-3 days"`. |
| `highlights` | event  | Optional array of short strings — what to look for / do. Rendered as sub-bullets on the card. Omit or use `[]` if none. |
| `note`     | event    | Optional. Extra context; omit the field if not needed. |
| `prereq`   | event    | Optional. Advance prep / prerequisite — e.g. a permit you must apply for ahead of time (Angels Landing lottery, timed-entry reservation). Rendered as a highlighted "⚠ Plan ahead" badge. Omit if nothing needs arranging in advance. |

A single place can have several events with different `months` (e.g. Yosemite's
firefall in February vs. high-country hiking in summer). Each event renders into
every month it lists; `"all"` events render in the separate year-round band.

### Two views

The page has a **View** toggle:

- **By month** — the 12-month grid + year-round band. A **Compact** display
  toggle (on by default) shows each multi-month event only in its first month to
  reduce clutter, while the caption still lists every month it spans (e.g.
  "Apr, May, Jun"). Turn Compact off to repeat the event in every month it spans.
  Clicking a place name on any event jumps to that place in the by-place view.
- **By place** — one card per place gathering *all* its events (sorted by month)
  with a month-coverage strip, so you can see every way to experience a place
  across the year. Clicking a place card header focuses just that place.

This is why multi-visit places should be modeled as one `place` with several
`events` (rather than duplicate place entries) — the by-place view groups them.

Each event card shows the **place** as the main heading, the event **title** as a
subheading, then a meta row (category, trip-type, duration, months) and the
**highlights** as sub-bullets. Beyond the view toggle there are **Categories**
(nature / city) and **Trip type** (multi-day / day trip) filter toggles.

## Editing rules

- **Only edit the `DATA` array.** Add, remove, or modify place/event entries there.
- **Leave the HTML, CSS, and JavaScript below the `DATA` array intact.** The
  rendering and interaction code does not need to change to add data.
- After editing, just refresh the browser — no build or reload tooling required.

## Commands

- `/add-bucket-list <place>` — research the best time(s) of year to visit a place,
  propose events for confirmation, and (after you confirm) merge them into the
  `DATA` array. See [`.claude/skills/add-bucket-list/SKILL.md`](.claude/skills/add-bucket-list/SKILL.md).
