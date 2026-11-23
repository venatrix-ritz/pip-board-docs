# Rules

A rule — the editor calls it a **policy** — is a trigger, some branches, and a fallback.

```
> POLICY  "Messages"
> TRIGGER  app ∈ { Messages }
  > BRANCH  "Amanda"
    > MATCH  title is "Amanda"
    > DO     speak
  > BRANCH  "Work"
    > MATCH  text contains "ticket"
    > DO     remind 15m×3, speaks
> ELSE
    > SILENCE
```

The trigger decides which notifications the rule looks at. Branches run in order, and the first one
whose conditions match wins. If no branch matches, the ELSE runs. Tap a branch's name to rename it, move it up or down, or delete it.

Each card on the rules list says what its rule does, for example
`DISCORD · speaks · reminds every 5 min ×3 (no speech)`. Open a rule to change it; its POLICY sheet
has **duplicate** (opens an unsaved copy named "<name> copy") and **delete policy**. Leaving the editor
with unsaved changes asks first: SAVE, DISCARD or KEEP EDITING.

A new rule starts from a preset: **speak everything from an app** and **silence an app** (both at
priority -100, so specific rules for the same app get first look), **speak only from a person**,
**remind me until I deal with it** (speaks, every 15 min until handled, pinned), or **blank**.
To start from a real notification, long-press it on the board's policy view so it is the only one
selected, then tap **new rule from this** (⊕): the rule opens with that app, and a branch that speaks
notifications with that title.

---

## Triggers

The trigger is an app filter. Pick one or more apps; leave it empty to match every app.

An empty trigger plus a broad branch is how you write a catch-all rule — see
[priority](#priority-and-order) for how to keep it from eating everything.

---

## Conditions

Each branch holds one or more conditions. All of them must be true for the branch to match.

### Content

Matches text in the notification. Choose which fields to look at:

| Field | What it usually holds |
|---|---|
| Title | Sender name, subject line, app-defined heading |
| Text | The message body — the main line you see |
| Subtext | Secondary line: account name, folder, category |
| Big text | The expanded body, when the notification has one |

Title and text are checked by default. Three matching modes, all **case-insensitive**:

- **Contains** — plain substring. The default.
- **Exact** — the whole field must equal your query.
- **Regex** — full regular expression. A pattern that fails to compile simply never matches rather
  than breaking the rule, so a typo makes a condition go quiet rather than crash anything.

A condition passes if *any* of its selected fields match, so ticking title and text means "either
one contains this".

Any condition can be **negated**, turning "contains" into "does not contain". This is how you write
an ignore rule: match the app, negate a content condition, and the branch fires for everything
*except* what you named.

### Channel

Matches the notification channel the app posted to — the same channels you see under an app's
notification settings. Useful when an app is well-behaved enough to separate its own traffic, since
channels are stable in a way that message text isn't.

### Category

Matches Android's own classification: message, email, alarm, call, transport, and so on. Apps set
this themselves and many get it wrong or leave it blank, so treat it as a hint, not a guarantee.

### AND / OR / NOT

Conditions can nest. Group several into an AND (all must match) or an OR (any may match), and wrap
anything in a NOT to invert it.

To match one of several senders, open a match and use **or also**: the match becomes
`title contains any of: Sender · Colleague · Shop`, with ✕ on each extra name. Two branches that do
exactly the same thing, each with one such match, can be joined from the lower one's BRANCH sheet
(**merge into "…"**); the merged branch keeps the upper one's name.

---

## Actions

### Read aloud

Speaks the notification. Choose which fields get read, or write a template to control the wording
exactly — see [Read-aloud](read-aloud.md).

### Remind

Pins the notification and re-alerts on a schedule.

| Option | Meaning |
|---|---|
| Interval | Minutes between repeats |
| Max repeats | How many times before it gives up — set it to 0 to repeat until you deal with it (the DO line says "until handled") |
| Sound | Custom sound, or silent |
| Vibration | Named vibration pattern |
| Says | **same as alert** (the default for a new reminder: the reminder reads what the first alert reads, and follows it when that changes; if the alert doesn't speak, it reads the app name, title and text), **custom** (its own read-aloud settings), or **nothing** |
| Persistent | Survives being swiped away — it comes back until you deal with it in ProBoard |

The collapsed DO / ELSE line spells the reminder out, for example `speak · remind 5m×3, speaks, pinned`
or `remind 5m×3, no speech`. A `!!` in front of it marks a reminder that rings without speaking
while the alert itself speaks, or one that is silent and says nothing; the Actions sheet says which.

On the debug build the Actions sheet also shows what the alert and its reminder will say, as
`will say: "…"`, built from the newest captured notification that branch would claim, so a
wording change shows its effect before you save.
**▶ try it** plays that line through the same audio queue as a real alert, and **▶ try the reminder**
plays the reminder's tone, vibration and speech. Both follow your speech settings, so with
Bluetooth-only on they stay quiet until Bluetooth audio is connected.

Persistent reminders are the reason the app exists. A normal notification is gone the moment you
brush it off the lock screen; a persistent one isn't.

### Silence

Consumes the notification and does nothing else.

This isn't the same as having no action. A branch that silences still **counts as a match**, which
stops rule evaluation — so lower-priority and catch-all rules never see that notification. It's how
you carve one sender out of a broad rule:

```
> POLICY  "Email"          priority 0
  > BRANCH "Newsletters"  → SILENCE
  > ELSE                  → read aloud
```

---

## Priority and order

Every rule has a **priority**. Rules evaluate from highest to lowest, and by default the first rule
that matches stops the rest from running.

The convention that keeps this manageable:

- Specific rules — one sender, one keyword — stay at the default priority of **0**.
- Catch-all rules that match a whole app with no content condition go to **-100** so they always
  evaluate last.

Without that, two rules at the same priority break the tie by internal ID, which is effectively
creation order — meaning a broad rule you made early will silently shadow a specific rule you made
later. If a rule that looks correct never fires, this is the first thing to check.

A rule that matches but resolves to no actual action is skipped and doesn't stop evaluation, so an
empty branch won't accidentally swallow a notification.

---

## Per-rule capture toggles

Three switches on each policy, under **advanced** in its POLICY sheet, control whether it's even allowed to see certain classes of
notification. All three default to **off**.

| Toggle | Off (default) | On |
|---|---|---|
| Capture ongoing | Ignores ongoing notifications | Sees media players, navigation, downloads, foreground services |
| Capture silent | Ignores low-importance notifications | Sees anything the system marked silent |
| Capture summaries | Ignores group summaries | Sees the "N new messages" roll-up headers |

These exist because those three categories generate enormous noise. Media players repost every few
seconds; group summaries carry no real content of their own.

### Empty summaries are always dropped

One case ignores the toggle entirely: a group summary carrying no content of its own — no per-item
lines, nothing but an app name or an account address — is discarded no matter what **Capture
summaries** is set to.

There's nothing to read out. Before this was filtered, such notifications would match a rule and
get spoken as just the app's name.

---

## Repeats

Apps repost the same notification constantly — on every message in a thread, on every progress
update, sometimes on a timer. ProBoard filters those so a rule doesn't fire over and over for one
event.

A repost is treated as the same event when its content fingerprint is unchanged. In that case the
Board entry updates, but actions don't re-fire and the rule's match count doesn't increase. Change
the content — a new message in the thread — and it counts as a new event.

Two additional guards catch reposts the fingerprint comparison alone would miss: a one-second
window that absorbs rapid-fire updates, and a longer-lived record that recognises a repeat even if
the notification's identity shifts between posts.

Note that clearing a notification from the system shade counts as dealing with it — the next repost
after that is a fresh event and will act normally.
