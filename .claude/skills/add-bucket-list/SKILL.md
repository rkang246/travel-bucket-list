---
name: add-bucket-list
description: Research the best time(s) of year to visit a place and add it to the bucket-list DATA array. Invoked as /add-bucket-list <place>.
argument-hint: <place>
allowed-tools: Read, Edit, WebSearch, WebFetch
---

# /add-bucket-list <place>

Add a place to the experiential bucket list in `bucket-list.html` by researching
when it is best to go, then merging it into the `DATA` array.

`$ARGUMENTS` is the place to add (e.g. `Patagonia`, `Iceland`, `Grand Canyon`).

Read [`CLAUDE.md`](../../../CLAUDE.md) first if you are unsure of the data model.
The model is **place → events**, where each event has `title`, `months` (array of
`1`–`12`, or `"all"`), `category` (`nature` | `city`),
`duration`, an optional `note`, and an optional `prereq` (advance prep, such as a
permit you must apply for ahead of time).

## Workflow

### 1. Research
Use `WebSearch` (and `WebFetch` to read promising results) to find the optimal
time(s) of year to experience the place.

- **Prefer one trip. Minimize repeat visits.** The user wants to see as much of a
  place as possible per trip. Before splitting anything, look for a single time
  window in which most or all of the worthwhile experiences are good at once
  (e.g. at Zion, the Narrows and Angels Landing both work in spring/early fall —
  that is *one* trip, so prefer overlapping months on a single event or a small
  set of events sharing the same window). Treat finding that optimal overlap as
  the main goal.
- **Split into separate events only when genuinely necessary**, such as:
  - the experiences peak in **non-overlapping seasons** (e.g. Yosemite firefall in
    February vs. summer high-country hiking — no single trip covers both), or
  - the place is **so large that one trip can't reasonably cover it** (e.g.
    Hawaii's different islands, or a big region needing multiple bases).
  When you do split, prefer the **fewest events** that still capture the distinct
  trips — don't fragment a single feasible trip into many entries.
- **When it's a judgment call, discuss it.** If experiences *mostly* overlap but
  not perfectly, surface the trade-off in the proposal (step 2) and suggest the
  window that captures the most in one trip, rather than silently splitting.
- Convert seasons/months you find into month numbers `1`–`12`.
- Only flag an experience as `months: "all"` if it is **genuinely year-round**
  (good in essentially every month) — not merely "open all year."
- Pick a sensible `category` (`nature` or `city`) for each event and estimate a
  `duration`. Add a short
  `note` when there is a useful caveat (e.g. "peak bloom is short, late March").
- **Research prerequisites.** Check whether the experience needs anything arranged
  in advance — a permit or lottery (e.g. Angels Landing, Half Dome cables, Inca
  Trail), timed-entry reservation, guide/outfitter booking, or similar. If so,
  capture it in `prereq` with enough detail to act on (what to get, where, and
  roughly how far ahead). Omit `prereq` when there is genuinely nothing to arrange.

### 2. Propose and pause
Present the proposed place and its events as a clear summary (place, region, and a
list of events with their months, category, duration, notes, and any **prereq /
advance prep** — call these out clearly so the user knows what to arrange ahead of
time).

- Make clear **how many trips** the proposal implies and **why**. If everything
  fits one trip, say so and give the recommended window. If you split, briefly
  justify each split (different season / too large to cover at once).
- If it was a judgment call, present the consolidation option alongside the split
  option and recommend the one that minimizes repeat trips.

**Stop and ask the user to confirm before editing anything.** Do not touch
`bucket-list.html` yet.

### 3. Merge (only after confirmation)
Once the user confirms:

1. `Read` `bucket-list.html` and locate the `DATA` array.
2. If a place with the same name already exists, **merge** the new events into
   that place's `events` array. Otherwise, **append a new place block** at the end
   of the array, in the exact shape:

   ```js
   {
     place: "Place Name",
     region: "Region",
     events: [
       {
         title: "Experience",
         months: [4, 5],
         category: "nature",
         duration: "2-3 days",
         note: "Optional note.",
         prereq: "Optional advance prep, e.g. permit lottery on recreation.gov."
       }
     ]
   }
   ```

3. **Touch only the `DATA` array.** Do not modify the HTML, CSS, or JavaScript.
   Match the existing indentation and trailing-comma style.

### 4. Confirm and offer commit
- Tell the user exactly what was added (place + event titles/months).
- Tell them to **refresh the browser** to see the changes.
- Offer to git-commit the change.
