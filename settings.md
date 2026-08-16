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
| **Snooze Duration** | 5 min | How long snoozing a reminder postpones it. |

Sync only considers reminders that are actually live. A rule that hasn't caught anything doesn't
affect the schedule.

---

## Timers & Alerts

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
| **Backup All Data** | Writes a `.json` file wherever you choose |
| **Restore All Data** | Loads a backup, replacing everything currently in the app |
| **Clear Captured History** | Empties the timeline; rules and settings untouched |
| **Clear Routine Logs** | Empties routine completion history |
| **Delete All Data** | Removes all captured notifications |
| **Reset All Settings** | Returns every setting to default; rules untouched |
| **Reset Onboarding** | Shows the setup walkthrough again on next launch |

Every destructive action asks for confirmation first.

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
