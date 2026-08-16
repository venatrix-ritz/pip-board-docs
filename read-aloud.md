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

**Engine.** Settings lets you pick which installed TTS engine to use, if you have more than one.
Voice, speed and pitch come from that engine's own system settings.

---

## Reminders can speak too

A repeating reminder has its own independent read-aloud setting with its own template. You can have
a notification read once when it arrives, then have the reminder say something shorter each time it
nags — or stay silent and just show up.
