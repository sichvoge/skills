---
name: conference-to-calendar
description: Extract a conference/event program (from a webpage, PDF, or pasted text) into a calendar file (.ics) the user can import into Google Calendar, Outlook, or Apple Calendar. Use this whenever the user shares a conference agenda, program page, or schedule and wants it "on my calendar," "as a calendar," or wants to "import" or "add" sessions to a calendar. Also trigger on phrases like "make me a calendar from this program" or "add this agenda to Google Calendar."
---

# Conference Program → Calendar

Turns a conference program (web page, PDF, or pasted schedule) into an `.ics`
calendar file the user can import into whatever calendar app they use.

## Why .ics instead of calling a calendar connector directly

Check the available tools first. If a live, already-connected calendar tool
(Google Calendar, Outlook, etc.) is actually present, it's fine to create
events directly with it — confirm the event list with the user first, same as
below.

If no such connector is available (common in claude.ai chat, which is not
the same as the MCP servers listed for the "Claude in Claude" API artifact
feature — that list does NOT mean the tool is callable here), don't tell the
user you can't help. Generate an `.ics` file instead. It's a universal
format every major calendar app can import, and it's arguably better for
bulk multi-session imports than one-by-one API calls anyway.

## Workflow

1. **Get the source material.** If the user gives a URL, `web_fetch` it. If
   they upload a file, read it (PDF/image/text). If it's pasted text, use it
   directly. Don't guess at a program you haven't actually seen.

2. **Check granularity before building anything.** Programs vary a lot in
   density — a single-track 2-day conference might have 25+ sessions
   including breaks; a multi-track one might have too many parallel options
   to sanely represent as one calendar. Ask the user (via
   `ask_user_input_v0` if available, otherwise a plain question):
   - One event per talk/session (most granular, best for single-track)
   - One event per day block (least granular, good for large/multi-track)
   - A mix: keynotes/panels as their own events, everything else grouped
   If the program has multiple parallel tracks, also ask which track(s) the
   user actually wants — don't add sessions they won't attend.

3. **Extract session data.** For each event, pull:
   - Title, speaker(s) and affiliation if given
   - Date and start time (and end time if stated)
   - Location/venue
   - Any other program-specific detail worth keeping (moderator, panelists,
     sub-talks within a lightning-talk block, etc.) — put this in
     DESCRIPTION, not SUMMARY

4. **Infer missing end times explicitly, and say so.** Programs frequently
   list only start times. Default to "ends when the next item starts."
   Flag any inference (missing end times, ambiguous durations, timezone
   assumptions) to the user in the final response — don't silently invent
   precision the source didn't have.

5. **Build the .ics file.** See `reference.md` for the exact format,
   escaping rules, and a full working example. Key points:
   - One `VEVENT` per session, sequential `UID`s
   - Use `DTSTART;TZID=<IANA tz>:` / `DTEND;TZID=<IANA tz>:` with local
     time, not UTC — figure out the event's timezone from the venue city
   - `SUMMARY` = short title (speaker + talk title is usually right);
     `DESCRIPTION` = extra detail (panelists, sub-agenda, abstract if short)
   - `LOCATION` = venue name (a room/venue-per-day is fine if that's all
     you have)

6. **Save and present the file.** Write to `/mnt/user-data/outputs/`, then
   `present_files`. Never just print the contents in chat — the user needs
   an actual downloadable file.

7. **Tell the user how to import it** (one line, not a full tutorial):
   Google Calendar → Settings → Import & export → Import. Outlook and
   Apple Calendar both support File → Import for `.ics` too.

8. **Flag inferences and gaps** in the reply: missing end times, assumed
   timezone, any sessions skipped (e.g. parallel tracks not selected).

## What NOT to do

- Don't fabricate a program you weren't given — always fetch/read the
  actual source first.
- Don't silently drop the granularity question and pick one yourself if the
  program is non-trivial (a handful of sessions is fine to just do; 15+ or
  multi-track is not).
- Don't claim a calendar event was "added to your Google Calendar" unless a
  real connected calendar tool was actually called — an .ics file is
  "ready to import," not "added."

See `reference.md` for the full ICS format reference and example file.
