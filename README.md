# celcat-ics-subscription

Turns a public CELCAT timetable (Université de Bordeaux) into an `.ics` feed you can subscribe to.

CELCAT shows group timetables on the web but offers no `.ics` for them. This script calls the same endpoint the page uses (`POST /calendar/Home/GetCalendarData`) and writes iCalendar.

## Usage

Python 3.9+, standard library only.

```
python3 celcat_ics.py --group "4TVL904S M2 Algorithms, Models and Verification" -o calendar.ics
```

- `--group` — the exact group id, from the `fid0` parameter of the CELCAT URL
- `--start`, `--end` — `YYYY-MM-DD`
- `--exclude` — skip a module, by code or name; repeatable
- `-o` — output file

A GitHub Actions workflow regenerates the file on a schedule and commits it only when it changes. Cron times are UTC, so they shift by an hour between summer and winter.

## Subscribing

... works only for my specific case though, an AMV student at Université de Bordeaux who does not follow Logic & Languages course.

```
https://raw.githubusercontent.com/Hizaak/celcat-ics-subscription/main/calendar.ics
```

Apple Calendar refreshes roughly hourly, Google Calendar much less often. Subscribed calendars are read-only.

## Notes

CELCAT descriptions are HTML with an inconsistent field order, so module, room, staff and weeks are split heuristically. Some of them also contain bare carriage returns, which have to be stripped: a lone `CR` inside a value makes the file unreadable for strict clients such as Google Calendar, while others accept it silently.

Times are written in local time with a `VTIMEZONE` block for `Europe/Paris`.
