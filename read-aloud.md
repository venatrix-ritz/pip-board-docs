# Read-aloud

Any branch can speak its notification through your device's text-to-speech engine.

There are two ways to control what gets said: pick which fields to read, or write a template.

---

## Field mode

The simple option. Tick the fields you want spoken and they're read in order, separated by pauses.

| Field | Read by default |
|---|---|
| App name | no |
| Title | yes |
| Text | yes |
| Subtext | no |
| Big text | no |

With the defaults, a message notification is spoken as *"Amanda. Are you home yet"* — title, then
body.

Turn on **app name** when a rule covers several apps and you need to know which one it came from.
Leave **big text** off unless you actually want long bodies read in full; it's usually the same
content as text, just longer.

---

## Template mode

Write a template and it replaces field mode entirely. Placeholders get substituted with the
notification's content:

| Placeholder | Value |
|---|---|
| `{app}` | App name |
| `{title}` | Title |
| `{text}` | Body text |
| `{subtext}` | Subtext |
| `{bigtext}` | Expanded body |

```
{title} says {text}
```

→ *"Amanda says are you home yet"*

Anything that isn't a placeholder is spoken literally, so you can add your own wording:

```
Message from {title}. It says, {text}
```

---

## Regex extraction

A placeholder can carry a regular expression, and then only the matching part is spoken:

```
{placeholder:pattern}
```

This is the feature that makes read-aloud genuinely useful for codes and numbers.

| Template | Notification | Spoken |
|---|---|---|
| `Your code is {text:\d{6}}` | "Your verification code is 482910" | "Your code is 482910" |
| `{text:\d+}` | "3 new items, 2 pending" | "3. 2" |
| `{title:^\w+}` | "Amanda Wilson" | "Amanda" |

If the pattern finds several matches they're all spoken, separated by pauses. Turn on
**Read last match only** to speak just the final one — useful when a message quotes an earlier code
before giving the current one.

If the pattern matches nothing, the placeholder resolves to empty and the rest of the template still
speaks. If the pattern is invalid, the field's full text is used rather than failing.

---

## Playback behaviour

**Nothing overlaps.** Read-alouds, reminder speech, and the hourly chime queue and play one at a
time. A notification arriving mid-sentence waits its turn rather than talking over what's already
being said.

**Music pauses and resumes.** ProBoard requests audio focus only for as long as it's speaking, and
releases it properly afterwards, so whatever you were listening to comes back on its own.

**Engine.** ProBoard picks a text-to-speech engine automatically on first use, preferring Google's,
then Samsung's, then whatever else is installed. There's no engine picker in Settings — voice,
speed and pitch all come from that engine's own settings under Android's
**Settings → Accessibility → Text-to-speech output**.

**Bluetooth only.** Off by default. Turn it on (Settings → Notifications → Read Aloud) and
notifications are only spoken while a Bluetooth audio device is connected — headphones, earbuds, a
car stereo. Anything arriving while you're disconnected is captured to the Board silently instead.
The check runs per notification, so unplugging mid-day takes effect immediately.

---

## Getting email to read properly

Email is the one place where picking the right field matters, because mail apps don't use them the
way you'd guess:

| Field | What an email actually puts there |
|---|---|
| Title | The **sender** |
| Text | The **subject line only** |
| Big text | The subject *and* the body |

So a rule reading title + text announces the sender and the subject, then stops — which reads as
the notification being cut off mid-way. To hear the actual message, turn **big text** on.

Because big text repeats the subject at its start, leaving **text** on as well makes the subject
get read twice. The combination that works is **title + big text, with text off**: sender, then
subject, then body, each once.

Messaging apps are the opposite — they rarely set big text at all, and when they do it just repeats
the body. Leave big text off for those, or you'll hear every message twice.

One limit that isn't ProBoard's to fix: some mail apps truncate the body they hand to notifications,
often around 250 characters. When that happens the message is already cut off before ProBoard sees
it, so a long email may still end mid-sentence.

---

## Reminders can speak too

A repeating reminder has its own independent read-aloud setting with its own template. You can have
a notification read once when it arrives, then have the reminder say something shorter each time it
nags — or stay silent and just show up.
