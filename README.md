# Pip-Board User Guide

**Pip-Board** is a smart notification filter and focus assistant for Android. It sits between you and your phone's notification stream — capturing alerts based on rules you set, reading them aloud, and reminding you of the ones that matter.

> **Scope:** This guide covers the standard Pip-Board release. That build centers on the notification workflow — the **Board**, **Rules**, **History**, and **Settings** (plus home-screen widgets). Additional modules that appear in development builds — timers, tasks, daily routines, and flashcards — are not part of this release, so this guide does not cover them.

---

## Table of Contents

1. [First-Time Setup](#1-first-time-setup)
2. [The Board — Your Notification Inbox](#2-the-board--your-notification-inbox)
3. [Creating Your First Rule](#3-creating-your-first-rule)
4. [Conditions — What to Match](#4-conditions--what-to-match)
5. [Branches — Multiple Scenarios in One Rule](#5-branches--multiple-scenarios-in-one-rule)
6. [Effects — What Happens on a Match](#6-effects--what-happens-on-a-match)
7. [Read Aloud Templates](#7-read-aloud-templates)
8. [Repeat Reminders](#8-repeat-reminders)
9. [Notification History](#9-notification-history)
10. [Global Capture Settings](#10-global-capture-settings)
11. [Backup and Restore](#11-backup-and-restore)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. First-Time Setup

After installing Pip-Board, the app walks you through a short setup sequence. Here is what each step requires and why:

### Notification Access
This is the core permission. Without it, Pip-Board cannot see incoming notifications and nothing will work.

1. Tap **Grant Notification Access** on the onboarding screen.
2. Find **Pip-Board** in the list and toggle it on.
3. Confirm the system warning — this is expected for any notification listener app.

> **Android 13+ note:** If you sideloaded the app, Android may block this permission until you allow it manually. Go to **Settings → Apps → Pip-Board → ⋮ (three-dot menu) → Allow restricted settings**, then come back and grant notification access.

### Post Notifications
Required so the app can send reminder alerts and chime notifications to you.

Tap **Allow** when Android prompts you during onboarding.

### Full-Screen Alerts *(optional)*
Needed only if you want alarm overlays to appear over the lock screen.

Tap **Grant** on the onboarding screen, then enable **Display over other apps** for Pip-Board in system settings.

---

## 2. The Board — Your Notification Inbox

The **Board** is the main screen. Every notification that matches one of your rules lands here, organized by rule and branch.

```
▼ Gmail                        ← rule name (tap to expand/collapse)
    ▼ OTP                      ← branch name (tap to expand/collapse)
        Verification code: 482910      2:14 PM
        Verification code: 771034      2:31 PM
    ▼ Invoice
        Your receipt from Stripe       1:05 PM
```

### Board Actions

| What you want | How |
|---|---|
| Expand or collapse a rule group | Tap the rule header row |
| Expand or collapse a branch | Tap the branch chip |
| Stop reminders on a capture | Tap the stop icon on the entry |
| Clear a single capture | Swipe it left or right |
| Clear everything | Tap **Clear All** in the top bar |
| Create a rule based on a capture | Tap the capture → rule editor opens pre-filled |
| Pin an important capture | Long-press → pin (stays at top until cleared) |

The **Master Switch** in Settings controls the entire app. When it is off, no notifications are captured, no reminders fire, and no chimes play.

---

## 3. Creating Your First Rule

Rules tell Pip-Board which notifications to capture and what to do with them.

1. Open the **Rules** tab.
2. Tap the **+** button (top right).
3. Give the rule a name — something descriptive like *Work Slack* or *Bank Alerts*.
4. Set the **App** — tap the app picker and choose the source app.
5. Add at least one **Condition** to narrow down which notifications match (optional — leaving conditions empty matches every notification from that app).
6. Add at least one **Effect** — what the app does when a match fires.
7. Tap **Save**.

The rule is active immediately. Toggle the switch on the rule list to enable or disable it without deleting it.

---

## 4. Conditions — What to Match

Each condition checks one field of the incoming notification.

| Field | What it checks |
|---|---|
| **Package** | The app's package name (e.g. `com.google.android.gm`) |
| **Title** | The notification title line |
| **Body** | The main notification body text |
| **Subtext** | Secondary line — often the account name or sender |
| **Big Text** | The expanded notification content |
| **Channel** | The notification channel ID set by the sending app |
| **Category** | Android system notification category |
| **Ongoing** | Whether the notification is a persistent / ongoing type |
| **Silent** | Whether the notification was posted without sound |

### Match Modes

| Mode | Behavior |
|---|---|
| **Contains** | The field includes your text (case-insensitive) |
| **Exact** | The field matches your text exactly |
| **Regex** | The field matches your regular expression |
| **NOT** | Prefix to any mode — inverts the result |

**Example:** To catch Gmail OTP messages, set field to *Body*, mode to *Contains*, value to `verification code`.

Multiple conditions on the same branch use **AND** logic — all must match.

---

## 5. Branches — Multiple Scenarios in One Rule

A rule can have more than one **branch**. Each branch has its own conditions and its own effects. Pip-Board checks branches top-to-bottom and fires the first one that matches.

This lets one rule handle different cases cleanly:

```
Rule: Gmail
  Branch "OTP"      → body contains "code" OR title contains "verification"
  Branch "Invoice"  → body contains "invoice" OR body contains "receipt"
  Branch "Default"  → (no conditions — catches everything else)
```

The matched branch name appears as a label on the captured entry in the Board. When you tap a capture to create a new rule from it, the branch name is carried over automatically.

---

## 6. Effects — What Happens on a Match

Each branch can have one or more effects:

| Effect | What it does |
|---|---|
| **Capture** | Saves the notification to the Board (always on by default) |
| **Read Aloud** | Speaks the notification using the device's text-to-speech engine |
| **Repeat Reminder** | Keeps alerting you at an interval until you clear the capture |
| **Silence (block fallback)** | Matches and consumes the notification silently — no sound, no read, no reminder — and stops any lower-priority or fallback rule from also acting on it |

Combine effects freely — for example, read aloud AND set a repeat reminder on the same branch.

### Silence (block fallback)

Use **Silence** when you want a specific notification handled by *nothing* — not even a broad fallback rule.

**Example:** You have a catch-all *Email (Fallback)* rule that reads every email aloud, plus a specific rule for work-order emails you no longer want announced. Instead of deleting the specific rule (which would let the fallback read the whole email), turn on **Silence** for its branch. Now those emails are captured to the Board quietly, and the fallback never touches them.

- Turning on Silence overrides Read Aloud and Reminders for that branch.
- A rule with a Silence branch shows a **SILENT** tag on the Rules list.
- Notifications silenced this way appear in **History** with a mute icon, so you can still see they arrived.

> **Audio never overlaps.** Read-alouds, reminder tones, and the hourly chime all play one at a time — a new notification's read waits for the current one to finish rather than talking over it.

---

## 7. Read Aloud Templates

When **Read Aloud** is enabled on a branch, you can control exactly what gets spoken.

### Placeholders

Write a template using these placeholders:

| Placeholder | Spoken content |
|---|---|
| `{app}` | Name of the source app |
| `{title}` | Notification title |
| `{text}` | Body text |
| `{subtext}` | Subtext line |
| `{bigtext}` | Expanded notification text |

**Example template:**
```
{app}: {title}. {text}
```
Spoken as: *"Gmail: New message. You have a verification code: 482910"*

### Regex Extraction

Extract and speak only part of a field using a regex pattern inside the placeholder:

```
{text:\d+}           ← speak only the digits found in the body
{title:urgent.*}     ← speak the part of the title matching "urgent..."
```

If the pattern has no match, that placeholder is silently skipped.

Leave the template blank to speak the full notification in a default format.

---

## 8. Repeat Reminders

Enable **Repeat Reminder** on a branch to keep alerting you until you manually clear the capture.

| Setting | What it does |
|---|---|
| **Initial delay** | How long to wait before the first reminder fires |
| **Interval** | Time between each subsequent reminder |
| **Max repeats** | Stop after N alerts — set to 0 for unlimited |
| **Sound** | Custom notification sound for reminders |
| **Silent** | Fire alerts without sound or vibration |
| **Read aloud** | Speak a custom message on each reminder alert |
| **Vibration pattern** | Custom vibration sequence |

To stop reminders early, tap the stop icon on the capture in the Board, or swipe to clear it.

### Sync Reminders

Reminders never overlap — they play one at a time. The **Sync Reminders** setting (Settings → Reminders, on by default) controls their *timing*:

- **On** — active reminders are batched onto a shared schedule so they tend to arrive together in one session instead of scattered through the day.
- **Off** — each active reminder fires on its own interval, whenever it comes due.

Sync is based on reminders that are actually active (a notification has triggered them) — a rule that hasn't captured anything yet doesn't affect the schedule.

---

## 9. Notification History

The **History** tab shows a log of every notification that has ever matched a rule, including ones you have already cleared from the Board.

Use history to:
- Review what was captured and when
- Check match counts per rule
- Look up old alert content you may have dismissed

---

## 10. Global Capture Settings

These settings apply to every rule before individual rule conditions are checked.

| Setting | Default | What it does |
|---|---|---|
| **App Active** | On | Master switch — off disables everything |
| **Capture Ongoing** | Off | Include persistent notifications (music players, navigation, etc.) |
| **Capture Silent** | Off | Include notifications posted without sound |
| **Exclude Group Summaries** | On | Skip Android bundle summary rows (reduces noise) |
| **Sync Reminders** | On | Batch active repeat-reminders onto one shared schedule so they arrive together; off, each fires on its own interval |
| **Use 24h Format** | Off | Show timestamps in 24-hour format across the app |
| **Hourly Chime** | Off | Play an audio chime or speak the time at the top of each hour |

### Hourly Chime Modes

- **Speak** — reads the current time aloud (e.g. *"It is 3 o'clock PM"*)
- **Beep** — plays a morse-style tone; adjust dit/dah lengths and gap to change the sound

Tap **Test Chime** in settings to preview before enabling.

---

## 11. Backup and Restore

Backups include: rules, Board captures, capture history, app sources, reminders, settings.

### Create a Backup
1. Open **Settings → Data & Backup**.
2. Tap **Export Backup**.
3. Choose where to save the `.json` file.

### Restore a Backup
1. Open **Settings → Data & Backup**.
2. Tap **Restore Backup**.
3. Select the `.json` file.

> Restoring replaces all current app data. Export first if you want to keep your existing setup.

---

## 12. Troubleshooting

### Rules not firing

Work through this checklist:

1. **App Active** is turned on in Settings.
2. Notification access is granted to Pip-Board (Settings → Special app access → Notification access).
3. The rule itself is enabled (green toggle on the Rules list).
4. The **App** on the rule matches the actual source app. Use the app picker rather than typing the package name manually.
5. Your conditions aren't too strict — try removing conditions temporarily to confirm the rule fires on any notification from that app.
6. Check the **Capture Ongoing** and **Capture Silent** switches — if the notification is persistent or silent and those are off, it will never match.
7. **Gmail tip:** Gmail often puts the sender name in **Subtext**, not Body. If your condition targets Body and it's not matching, try switching to Subtext.

### "Restricted settings" blocks notification access

Android 13 and newer can restrict high-privilege permissions for sideloaded apps. Fix:

1. Open Android **Settings → Apps → Pip-Board**.
2. Tap the **⋮** menu → **Allow restricted settings**.
3. Return to notification access and grant it.

### Reminders keep firing after clearing a capture

Reminders are cancelled when you clear a capture or tap the stop icon. If they persist:

- Make sure **App Active** is on — the cancellation path runs through the notification service.
- Force-stop and restart the app, then clear the capture again.

### Notification captured multiple times

If the same notification appears more than once on the Board, check whether the source app posts rapid updates (common with messaging apps and group chats). Pip-Board deduplicates by content within a short window, but heavily updated notifications (like live-updating timers) may still produce multiple entries. Use the **Capture Ongoing** switch to filter these out globally, or add a condition that excludes the noisy package.
