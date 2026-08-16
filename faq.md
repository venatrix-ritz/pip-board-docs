# FAQ

---

## My rule isn't firing

Work down this list — it's roughly in order of how often each one turns out to be the cause.

**1. Is another rule catching it first?**

The most common cause by far. Rules run in priority order and the first match stops the rest. A
broad rule made earlier will quietly shadow a specific rule made later, because ties break by
creation order.

Fix: give catch-all rules a priority of **-100** so they always run last, and leave specific rules
at 0. See [priority](rules.md#priority-and-order).

**2. Is it an ongoing, silent, or summary notification?**

Each rule ignores all three by default. Media players and navigation are ongoing; anything the
system marked low-importance is silent; "N new messages" headers are summaries. Turn on the matching
toggle in that rule's policy settings.

**3. Is App Active on, and is notification access still granted?**

Android sometimes revokes notification access after an app update without saying so. If it has, the
Board shows a **GRANT NOTIFICATION LISTENER ACCESS** banner with a FIX button. Settings →
Permissions also lists the current state of every permission.

**4. Is the rule itself enabled?**

Check the toggle on the rule list.

**5. Are your conditions too tight?**

Temporarily strip the branch down to a single loose condition and see if it fires. If it does, add
conditions back one at a time.

Watch out for fields: many apps don't put content where you'd expect. Email apps often put the
sender in **subtext** rather than title, and messaging apps sometimes leave title blank on group
chats. Turn on capture for a moment and look at what actually arrives on the Board.

**6. Is the app hiding the content?**

Some apps replace notification text with a placeholder when their own privacy setting is on.
ProBoard reads the real underlying message where one exists, but if an app genuinely doesn't include
the content, there's nothing to match.

---

## Something got read aloud that shouldn't have

Check which rule caught it — the Board entry names the rule and branch.

Usually it's a catch-all rule with no content condition doing exactly what it was told. Either
tighten it, or add a specific rule above it that **silences** the case you don't want, which
consumes the notification so the catch-all never sees it.

---

## It read out just the app name and nothing else

That's a group summary — the roll-up header Android shows above bundled notifications. They often
carry no content of their own.

Summaries with nothing in them are dropped automatically now, regardless of settings. If you're
still hearing one, it's a summary that *does* contain text (like "5 new messages"), which means a
rule has **Capture summaries** switched on. Turn it off for that rule.

---

## The same notification keeps repeating

Apps repost constantly — every message in a thread, every progress tick. ProBoard treats a repost
with unchanged content as the same event and doesn't re-fire actions for it.

If something genuinely repeats, the content is changing each time. A download at 41%, then 42%, is
technically new content on every post. Turn off **Capture ongoing** for that rule (it's off by
default) and progress notifications stop being considered at all.

Note that clearing the notification from your system shade resets things — the next post after that
counts as a fresh event by design, since you've dealt with the old one.

---

## A reminder keeps going after I cleared it

Clearing an entry in ProBoard cancels its reminders. Clearing the original notification from the
system shade does **not** — that's the entire point of pinning. A persistent reminder is meant to
outlive a careless swipe.

Clear it from the Board, or long-press the entry to select it and use **Stop reminders** — that
silences the nagging while leaving the entry on the Board.

---

## Can I stop a notification appearing on my phone at all?

No. ProBoard reacts to notifications, it doesn't intercept them — Android doesn't allow a listener
app to prevent another app's notification from being shown. **Silence** stops *ProBoard* from
acting on it; to stop it showing at all, turn it off in the source app or in Android's own
notification settings for that app.

---

## Does any of this leave my phone?

No. ProBoard doesn't declare a network permission, so it has no ability to send anything anywhere.
Notification content is stored in the app's private database, and Settings can wipe it. Backups are
written where you choose and contain only your own data.

---

## Notification access keeps turning itself off

Usually battery optimisation killing the listener service. Exclude ProBoard from battery
optimisation in Android's settings. ProBoard also runs a watchdog that notices when the listener has
died and alerts you rather than silently failing.

On Android 13+ sideloads, also confirm **Allow restricted settings** is still granted — see
[Getting started](getting-started.md#1-allow-restricted-settings).

---

## What happened to timers and flashcards?

Removed. Earlier development builds had a Pomodoro timer and a flashcard system; neither was used
and both are gone.

Tasks and Routines still exist but are switched off in release builds while they're finished.
