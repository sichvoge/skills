# ICS format reference

## Minimal file skeleton

```
BEGIN:VCALENDAR
VERSION:2.0
PRODID:-//<source>//Program Extract//EN
CALSCALE:GREGORIAN
METHOD:PUBLISH

BEGIN:VEVENT
UID:<unique-id>@<some-namespace>
DTSTAMP:<UTC timestamp of creation, e.g. 20260907T000000Z>
DTSTART;TZID=<IANA timezone>:<local YYYYMMDDTHHMMSS>
DTEND;TZID=<IANA timezone>:<local YYYYMMDDTHHMMSS>
SUMMARY:<short title>
LOCATION:<venue>
DESCRIPTION:<optional extra detail>
END:VEVENT

... repeat VEVENT per session ...

END:VCALENDAR
```

## Rules that matter

- **Timezone**: use `TZID=Europe/Berlin` (or whatever the venue's IANA zone
  is) with a local (floating, no `Z`) datetime. Google Calendar, Outlook,
  and Apple Calendar all resolve standard IANA TZIDs correctly without
  needing a full `VTIMEZONE` block bundled in.
- **UID**: must be unique per event within the file. `<slug>-<n>@<anything>`
  is fine — it doesn't need to be globally meaningful, just unique within
  the file.
- **DTSTAMP**: required by the spec; the creation time of the file is fine,
  doesn't need to be exact.
- **Line folding**: technically ICS wants long lines folded at 75 octets.
  In practice, Google/Outlook/Apple import unfolded lines fine as long as
  each property is on one logical line — don't worry about this unless a
  user reports an import failure.
- **DESCRIPTION with multiple lines**: use literal `\n` inside the value
  (not an actual newline) to represent line breaks, e.g.:
  `DESCRIPTION:Speaker A – Talk one\nSpeaker B – Talk two`
- **Escaping**: commas, semicolons, and backslashes inside values should be
  escaped with a backslash (`\,` `\;` `\\`). Rare in practice for talk
  titles, but keep it in mind for abstracts pulled verbatim from a program.

## Worked example (2 events)

```
BEGIN:VCALENDAR
VERSION:2.0
PRODID:-//Example Conf//Program Extract//EN
CALSCALE:GREGORIAN
METHOD:PUBLISH

BEGIN:VEVENT
UID:exampleconf-1@claude
DTSTAMP:20260907T000000Z
DTSTART;TZID=Europe/Berlin:20260910T091500
DTEND;TZID=Europe/Berlin:20260910T100000
SUMMARY:Jane Doe (Acme Corp) – Why Everything Is On Fire
LOCATION:Kulturbrauerei Berlin, Kesselhaus
END:VEVENT

BEGIN:VEVENT
UID:exampleconf-2@claude
DTSTAMP:20260907T000000Z
DTSTART;TZID=Europe/Berlin:20260910T100000
DTEND;TZID=Europe/Berlin:20260910T104500
SUMMARY:Coffee Break
LOCATION:Kulturbrauerei Berlin, Kesselhaus
END:VEVENT

END:VCALENDAR
```

## Generation approach

For anything beyond a handful of events, generate the file with a heredoc in
`bash_tool` rather than typing every VEVENT by hand in a `create_file` call —
faster and less error-prone for 15+ sessions. See the SKILL.md workflow for
where this fits (step 5).
