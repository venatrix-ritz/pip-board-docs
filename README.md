# ProBoard

**User guide and documentation.**

ProBoard is a notification rules engine for Android with a terminal-style interface.

Your phone decides what's worth interrupting you for. ProBoard takes that decision back. It watches
every notification your device posts, matches them against rules you write, and decides what happens
next — read it aloud, pin it to a board that won't let you forget it, or silently swallow it so
nothing else reacts.

Everything runs on-device. No accounts, no network calls, no analytics.

---

## Guide

| | |
|---|---|
| [Getting started](getting-started.md) | Install, permissions, first rule |
| [Rules](rules.md) | Everything a rule can match on and do |
| [The Board](board.md) | Board, timeline, pinning, widget |
| [Read-aloud](read-aloud.md) | TTS options and template syntax |
| [Settings](settings.md) | Every setting and what it changes |
| [FAQ](faq.md) | Why didn't my rule fire, and other common questions |

If something isn't behaving the way you expect, start with the [FAQ](faq.md) — most surprises come
down to rule order, the per-rule capture toggles, or a summary notification being filtered out.

---

## What it does

**Rules.** Match notifications by app, channel, category, or content — plain text, exact match, or
regex — and combine those with AND / OR / NOT. When a rule matches, it acts: speak it, pin it,
remind you about it, or consume it silently.

**Branches.** One rule can fork. Check a condition, do one thing if it's true and another if it
isn't. Branches are named, and the name follows the notification onto the Board so you can see which
path it took.

**Priority.** Rules run in order and the first match wins, so specific rules sit in front of broad
ones. A rule that only suppresses still counts as a match — it eats the notification so your
catch-all rules never fire on it.

**The Board.** Everything that matched lands here. Pinned items stay put until you deal with them —
they survive being swiped out of the system shade, and mirror to a home screen widget.

**Read-aloud.** Have notifications spoken to you. Read the fields as-is, or write a template and
pull out just the part you care about: `Code is {text:\d{6}}` speaks the six digits and nothing else.

**Reminders.** Pin something to a repeating reminder — set the interval, the repeat count, the
sound, and whether it's allowed to nag past a dismissal.

**Hourly chime.** Optional. Spoken time or a configurable duotone beep, with a sleep window so it
stays quiet overnight.

---

## Status

Pre-release. The notification workflow — Board, Rules, History, Settings, and the home screen
widget — is complete and in daily use.

Tasks and Routines are built but switched off in release builds while they get finished, so this
guide doesn't cover them. Earlier development builds also carried a Pomodoro timer and a flashcard
system; both were removed.

Requires Android 8.0 or newer.

---

## Privacy

ProBoard doesn't declare a network permission, so it can't phone home even by accident.
Notification content lives in the app's private storage and nowhere else, and Settings can erase all
of it whenever you want. Backups go where you put them and contain only your own data.

No analytics. No tracking. No ads.

---

*This repository holds documentation only. The app was previously called Pip-Board — hence this
repository's name.*
