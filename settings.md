# Settings

Settings is organised into collapsible sections. Each section remembers whether you left it open.

---

## Notifications

| Setting | Default | What it does |
|---|---|---|
| **App Active** | On | The master switch. Off means nothing is captured, no reminders fire, no chimes play. The app stays installed and your rules stay intact — it just stops acting. |

There are no global capture filters here. Whether a rule sees ongoing, silent, or summary
notifications is decided **per rule**, in the rule's own policy settings — see
[Rules](rules.md#per-rule-capture-toggles). Older versions had these as app-wide switches; moving
them onto individual rules means one noisy app doesn't force a decision on everything else.

---

## Reminders

| Setting | Default | What it does |
|---|---|---|
| **Sync Reminders** | On | Batches active repeating reminders onto a shared schedule so they arrive together in one burst rather than scattered across the hour. Off, each reminder runs on its own clock. |

Sync only considers reminders that are actually live. A rule that hasn't caught anything doesn't
affect the schedule.

When it's on, reminders land on whole clock positions for their interval — a 15-minute reminder
arrives at :00, :15, :30 or :45 — so a burst is predictable rather than tied to whenever the first
one happened to fire. A newly created reminder always waits at least five minutes, so it can't go
off almost immediately just because it was made just before a boundary.

*Snooze length moved to Timers & Alerts, since it applies to alarms rather than to these reminders.*

---

## Timers & Alerts

### Snooze and auto-dismiss

| Setting | Default | What it does |
|---|---|---|
| **Alarm Snooze Length** | 5 min | How long the Snooze button postpones a routine or task alarm. |
| **Auto-Dismiss After** | 10 min | How long a full-screen alarm waits with no input before giving up. |

Neither affects notification reminders — those repeat on each rule's own interval. What happens
when an alarm auto-dismisses (snooze again, or cancel) is chosen per routine, not here.

> These control the full-screen alarms used by Routines and Tasks, which aren't part of this
> release yet. The settings are visible, but there's nothing using them until those features ship.

### Hourly chime

Off by default. When on, marks the top of every hour.

| Setting | What it does |
|---|---|
| **Chime Mode** | *Speak* reads the time aloud; *Beep* plays a two-tone pattern |
| **Dit / Dah length** | Short and long tone durations, in milliseconds |
| **Gap length** | Silence between tones |
| **Chime Wait** | The sleep window — hours during which the chime stays silent |

In beep mode the hour is encoded in the tones: one long tone (a low A) per five hours, then one
short tone (a higher E) for each hour remaining. Eight o'clock is one long and three short; noon is
two long and two short.

### Routine alerts

Only relevant when routines are enabled.

| Setting | What it does |
|---|---|
| **Routine Tone** | Alarm sound for routine reminders |
| **Routine Vibration** | Whether routine reminders vibrate |
| **Vibration Pattern** | Which named pattern to use |

---

## Display

| Setting | Default | What it does |
|---|---|---|
| **Theme Color** | Phosphor green | Terminal colour: green, amber, white, cyan, red, or magenta. Applies everywhere, including the widget and full-screen alarms. |
| **Use 24h Format** | Off | 12- or 24-hour time throughout the app. |

---

## Data

| Action | What it does |
|---|---|
| **Auto-Backup** | On by default. Saves a restore point to the device once a day, keeping the last 7 |
| **Backup All Data** | Writes a `.json` file wherever you choose |
| **Restore All Data** | Loads a backup, replacing everything currently in the app |
| **Clear Captured History** | Empties the timeline; rules and settings untouched |
| **Clear Routine Logs** | Empties routine completion history |
| **Delete All Data** | Removes all captured notifications |
| **Reset All Settings** | Returns every setting to default; rules untouched |
| **Reset Onboarding** | Shows the setup walkthrough again on next launch |

Every destructive action asks for confirmation first.

### Auto-backup

Once a day, ProBoard writes a full backup to its own folder on your device and keeps the last
seven, so there's always a recent restore point without having to remember to make one.

Two things worth knowing. It's stored in the app's private folder, which Android deletes when an
app is uninstalled — so it protects you from mistakes *inside* the app, not from uninstalling it.
For copies that outlive the install, use **Backup All Data** and save somewhere of your own. And it
will never overwrite a good backup with an empty one: if the app's data looks empty but the last
backup wasn't, it skips that run rather than rotating the good copy out.

### What's in a backup

Rules, captured notifications, tasks, routines, routine logs, reminders, and your settings.

Backups are plain readable JSON — you can open one in a text editor to check what it contains.
Restoring **replaces** your current data rather than merging, so export first if you want to keep
what you have.

---

## Permissions

A live status list of what ProBoard has been granted, with a shortcut into the relevant system
screen for anything missing. If notification access gets revoked — which Android sometimes does
silently after an update — this is where it shows up, and the app will also warn you.

---

## About

Version number, a link to this documentation, and the privacy policy.
