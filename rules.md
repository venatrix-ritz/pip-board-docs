# Rules

A rule — the editor calls it a **policy** — is a trigger, some branches, and a fallback.

```
> POLICY  "Messages"
> TRIGGER  app ∈ { Messages }
  > BRANCH  "Amanda"
    > MATCH  title is "Amanda"
    > DO     read aloud
  > BRANCH  "Work"
    > MATCH  text contains "ticket"
    > DO     remind every 15m
> ELSE
    > SILENCE
```

The trigger decides which notifications the rule looks at. Branches run in order, and the first one
whose conditions match wins. If no branch matches, the ELSE runs.

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

Title and text are checked by default. Three matching modes:

- **Contains** — plain substring, case-insensitive. The default.
- **Exact** — the whole field must equal your query.
- **Regex** — full regular expression. Invalid patterns simply never match rather than crashing.

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
| Max repeats | How many times before it gives up; unlimited is allowed |
| Sound | Custom sound, or silent |
| Vibration | Named vibration pattern |
| Read aloud | Speak the reminder as well as showing it |
| Persistent | Survives being swiped away — it comes back until you deal with it in ProBoard |

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

Three switches on each policy control whether it's even allowed to see certain classes of
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
