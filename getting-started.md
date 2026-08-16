# Getting started

## Install

ProBoard needs Android 8.0 or newer.

Install the APK, then work through the permissions below. Android deliberately makes notification
access awkward to grant for sideloaded apps, so the order matters.

### 1. Allow restricted settings

On Android 13 and newer, a sideloaded app can't be given notification access until you unlock it:

**Settings → Apps → ProBoard → ⋮ (top right) → Allow restricted settings**

Skip this and the notification-access toggle in the next step will be greyed out with no
explanation.

### 2. Grant notification access

**Settings → Notifications → Special app access → Notification access → ProBoard**

This is the permission that lets ProBoard see notifications at all. Nothing works without it.

### 3. Allow notifications

Grant **Post notifications** when prompted on first launch. ProBoard needs this to show you
reminders — without it, pinned items still appear on the Board but can't alert you.

### 4. Optional: display over other apps

Only needed for full-screen routine alarms. **Settings → Apps → ProBoard → Display over other apps**.

---

## Check it's working

Open ProBoard. If notification access isn't granted, a **GRANT NOTIFICATION LISTENER ACCESS** banner
sits at the top of the Board with a **FIX** button that takes you straight to the right system
screen. No banner means the listener is connected.

That banner is worth remembering — Android sometimes revokes notification access after an app
update without telling you, and this is where it shows up.

---

## Your first rule

Say you want your phone to read out messages from one person, and stay quiet about everyone else.

1. Go to **Rules** and create a new policy.
2. **Trigger** — pick the messaging app.
3. **Branch** — add a condition matching the sender's name against the notification title.
4. **Action** — set the branch to read aloud.
5. Save.

Send yourself a test message. It should be spoken; messages from anyone else in the same app
shouldn't be, because they don't match the branch condition and the rule has no fallback action.

### Adding a fallback

If you *do* want something to happen for everyone else, use the **ELSE** node at the bottom of the
rule. It runs when no branch matched — a good place for a quieter action, like pinning to the Board
without speaking.

---

## Where to go next

- [Rules](rules.md) — the full set of things you can match on and do
- [The Board](board.md) — what happens to a notification after it matches
- [FAQ](faq.md) — if a rule isn't firing when you think it should
