# The Board

Everything a rule matched lands on the Board. It's the app's main screen and the reason it's called
ProBoard rather than a notification filter.

---

## How it's organised

The Board doesn't show a flat list. It groups by **policy**, then by **branch** inside that policy:

```
▼ Email Policy                              ← the rule that matched
    ▼ Amanda                                ← the branch that matched
        Are you home yet              8:14 PM
        Bringing dinner               8:31 PM
    ▼ Default                               ← no named branch matched
        Receipt from Stripe           7:05 PM
▼ Calendar Policy
    ▼ Default
        Lock the door                 8:00 PM
```

Tap a policy or branch header to collapse or expand it.

Entries with no branch name group under **Default**. That's what you'll see when a rule matched
through its fallback rather than a named branch, so it's a quick way to spot rules that are catching
more than you intended.

### Ordering

Both levels sort the same way: **anything pinned floats to the top**, and within that, most recent
first. A policy with a pinned entry anywhere inside it outranks one without, so the things that are
still demanding attention stay at the top of the screen rather than being pushed down by newer
noise.

---

## The two tabs

The Board screen has two views over the same captured data.

**DATA STREAM** is what's still live — everything not yet dismissed, grouped by policy and branch as
above. This is the working surface, and what the widget mirrors.

**TIMELINE** is today's activity, dismissed or not, bucketed by **hour and app**. Rather than listing
every notification individually it shows each app's activity per hour with a count, and how many of
those were pinned — so a chatty group thread reads as one entry with a number on it instead of forty
rows.

The timeline covers today only and rolls over at midnight. Dismissing something removes it from
DATA STREAM but never from the timeline, so the timeline stays an honest record of what arrived.

---

## Pinning

A pinned entry is one whose rule attached a **persistent** reminder. Pinning isn't a manual action —
it's a property of the rule that caught the notification, set on the reminder effect.

The important part: clearing the underlying notification from the system shade doesn't clear the
pin. Ordinary notifications vanish the moment you swipe them away, whether or not you actually dealt
with them. A pin doesn't — it stays on the Board and keeps re-alerting on its schedule until you
clear it inside ProBoard.

Pinned entries are marked with a pin icon, and the header of any group containing one carries the
icon too, so you can see there's something outstanding inside a collapsed group.

Anything not pinned behaves normally: dismiss the source notification and it leaves the Board.

---

## Selecting and clearing

**Long-press** an entry to select it. Once anything is selected, a plain tap adds or removes others,
so you can pick off several at once without long-pressing each one.

With a selection active, the toolbar offers:

| Action | Effect |
|---|---|
| Stop reminders | Cancels the repeating reminders on the selected entries, leaving the entries in place |
| Clear selection | Deselects everything; nothing is changed |

**Clear all** in the toolbar empties the Board in one go, after a confirmation prompt.

"Stop reminders" is the one to reach for when something is nagging you but you still want it visible
— it silences the reminder without pretending you've dealt with it.

Clearing cancels any reminders attached to those entries, so a cleared item won't come back later.
Nothing is removed from the timeline — clearing marks entries dismissed rather than deleting them.

---

## Widget

The alerts widget mirrors the Board to your home screen — live, without opening the app. It still
appears in Android's widget picker under the app's former name, **Pip-Board Alerts**.

It lists active entries and shows a count, tapping an entry opens the app, and it carries its own
clear-all button which cancels reminders exactly like clearing inside the app does.

The widget follows your theme colour and updates whenever the Board changes.
