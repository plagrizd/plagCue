# plagCue — Stream Alert Overlay Manager

> **Free Windows desktop app — add-on for [plagComms](https://github.com/plagrizr/plagcomms). Requires plagComms to function.**  
> Connect live stream events from Twitch, TikTok, and YouTube to custom video and audio alerts in OBS — no third-party alert service, no subscription, no browser extension required.

**Created by [plagrizr](https://plagrizr.com)**

---

## What is plagCue?

plagCue is a **stream alert overlay manager** built to work alongside [plagComms](https://plagrizr.com). It listens for real-time events from your live streams — subscriptions, donations, gifts, raids, TikTok likes, YouTube superchats, channel point redemptions, and more — and fires your own custom video or audio files as overlay alerts in OBS the moment they happen.

You bring the media. plagCue handles the triggering, the timing, the queuing, and the delivery straight into your OBS browser source. No uploading files to a third party. No paying for an alert service. No lock-in.

It works with **Twitch, TikTok, and YouTube Live simultaneously**, meaning if you multistream you get one unified alert system covering all three platforms at once.

---

## Who is it for?

- Streamers who want **full control** over their alert media and when it plays
- Multi-platform streamers tired of managing separate alert systems per platform
- Creators who want alerts that work on both **widescreen and vertical (portrait) canvases** — ideal for TikTok LIVE layout alongside a standard stream
- Anyone who wants to fire alerts based on **specific conditions**, not just "someone subbed" — like "5+ month sub, Tier 2 or above" or "cheer of 500+ bits"

---

## Key Features

### Multi-platform event support
Receive and react to events from **Twitch, TikTok, and YouTube Live** all in one place. Disable platforms you don't use to keep the interface clean.

### Flexible trigger conditions
Every alert can have as many conditions as you need. Filter by amounts, usernames, message content, sub tiers, boolean flags, or value ranges. All conditions must pass for the alert to fire — no coding required.

### Weighted alert pools
Multiple alerts can target the same event type. Assign each one a **weight** and plagCue randomly picks which one plays when that event fires — great for rotating different alert videos so your stream doesn't feel repetitive.

### Per-alert cooldowns
Set a cooldown in seconds on any alert so it can't fire again until the timer resets. Prevents the same alert from spamming during a raid or gift bomb.

### Playback queues
Assign alerts to named **queues**. Alerts in the same queue play one at a time in order. Alerts in different queues play simultaneously. Configure a delay between items in a queue and a maximum depth — extras get dropped cleanly when the queue is full.

### Horizontal and vertical canvas support
Each alert can target your **widescreen canvas**, your **portrait/vertical canvas**, or both — independently. Point one OBS browser source at `/overlay/h` and another at `/overlay/v` and plagCue handles routing automatically.

### Fixed or randomised placement
Place alerts at an exact position on the canvas, or let plagCue randomise placement within bounds you define. Optional safe zones let you exclude specific regions (like where your webcam sits) from random placement.

### Automatic video compatibility
Clips recorded on phones (iPhone, Android) are typically HEVC/H.265, which OBS browser sources cannot decode — they'll appear distorted and jittery on stream. plagCue detects incompatible formats automatically when you select a file and converts them to H.264 with one click. Your original file is never modified.

### Test without going live
The **Test Events** page lets you inject any event type with custom values — subs, gifts, raids, superchats, anything — and see your alerts fire in real time without needing an active stream.

### Debug page with condition tracing
The **Debug** page shows a live trace of every event received: which alerts were evaluated, which conditions passed or failed, which alert was selected, and why others were skipped. Essential for dialling in complex trigger logic.

### Self-hosted, local-only
plagCue runs entirely on your machine. Media files stay on your drive. Nothing is uploaded anywhere. The overlay server is a local HTTP server your OBS browser source connects to directly.

### System tray app
Minimises to the system tray instead of closing. Set it to auto-connect and auto-minimise on launch so it runs invisibly in the background during your stream.

---

## Supported Events

| Event | Twitch | TikTok | YouTube |
|---|:---:|:---:|:---:|
| Chat message | ✓ | ✓ | ✓ |
| Gigantified emote | ✓ | | |
| Bits / Cheer | ✓ | | |
| Subscription | ✓ | ✓ | |
| Gift sub / TikTok gift | ✓ | ✓ | |
| Follow | ✓ | ✓ | ✓ |
| Raid | ✓ | | |
| Channel point redemption | ✓ | | |
| Watch streak (Power-Up) | ✓ | | |
| TikTok like | | ✓ | |
| TikTok share | | ✓ | |
| YouTube Super Chat | | | ✓ |
| YouTube Membership | | | ✓ |

---

## How It Works

plagCue is a companion app to **plagComms** — plagrizr's free multi-platform chat overlay. plagComms handles the OAuth connections to each platform and normalises all incoming events into a consistent format. plagCue connects to plagComms over a local WebSocket and receives those normalised events in real time.

When an event arrives:

1. plagCue checks it against every enabled alert rule
2. Rules that match go through weighted random selection
3. The winning alert's media file is pushed to the built-in overlay HTTP server
4. Your OBS browser source receives a play command and overlays the media on your canvas

The whole pipeline runs on your machine. plagComms and plagCue both need to be running, but neither requires an internet connection beyond what your streaming platforms already use.

---

## Installation

Download the latest `plagCue.exe` from the [Releases page](https://github.com/plagrizd/plagCue/releases/latest).

Run it. No installer, no dependencies, no setup wizard.

> **Windows Defender / SmartScreen note:** On first launch Windows may show an "unrecognized app" warning because plagCue isn't code-signed yet. Click **More info → Run anyway** to proceed. This is normal for unsigned indie software.

---

## Setup

### 1. Install plagComms
plagCue requires [plagComms](https://plagrizr.com) to receive stream events. Install and launch plagComms first. You only need to do this once — plagComms handles all platform authentication.

### 2. Launch plagCue
Double-click `plagCue.exe`. It will appear in your system tray and open its main window.

### 3. Connect to plagComms
Go to the **Connect** page. The host and port default to `localhost` and `54473` — these are correct if plagComms is running on the same machine. Click **Connect**.

The status indicator at the top-left turns green when the connection is live.

### 4. Add OBS browser sources
In OBS, add a **Browser Source** for each canvas layout you use:

| Layout | URL | Set size to |
|---|---|---|
| Horizontal (16:9) | `http://localhost:54474/overlay/h` | Your canvas width × height (e.g. 1920 × 1080) |
| Vertical (9:16) | `http://localhost:54474/overlay/v` | Your canvas width × height (e.g. 1080 × 1920) |

You only need the layouts you actually stream to. Most streamers only need the horizontal one.

### 5. Create your first alert
Go to **Alerts** and click **+ Add Alert**. Give it a name, pick an event type (e.g. Follow), browse for a video or audio file, and click **▶ Test** to preview it in OBS immediately.

### 6. Optional: auto-connect on launch
In **Settings**, enable **Auto-connect** and **Start minimized** so plagCue connects to plagComms silently in the background every time you start your PC — no manual steps before going live.

---

## OBS Browser Source Tips

- Set the browser source to **exactly** the size of your canvas — plagCue positions alerts within that space
- Make sure **"Shutdown source when not visible"** is **off** — the source needs to stay connected even when your scene isn't active
- If alerts stop playing, right-click the browser source in OBS and click **Refresh** to re-establish the connection
- Only have the overlay URL open in OBS — keeping it open in a browser tab at the same time means both receive and play every alert (doubled audio)

---

## Condition Filtering Reference

Conditions let you filter exactly when an alert fires. Every condition must pass (AND logic) — an alert with no conditions fires on every matching event.

**Operators available:**

| Field type | Operators |
|---|---|
| Number (bits, months, viewer count, etc.) | at least, at most, exactly, not equal to, between |
| Text (username, message, reward name) | contains, starts with, exactly, not equal to |
| Boolean (first-time chatter, announcement, etc.) | is true, is false |
| Sub tier | exactly, not equal to (T1 / T2 / T3 / Prime) |

**Example conditions:**
- Fire a cheer alert only when `bits ≥ 100`
- Fire a sub alert only when `months between 3 and 6` and `tier = T2`
- Fire a redemption alert only when `reward name contains "hydrate"`
- Fire a raid alert only when `viewer count ≥ 50`
- Fire a chat alert only for `first-time chatters` whose message `contains "lurker"`

---

## Playback Queue Reference

| Setting | What it does |
|---|---|
| **Queue name** | Label shown in the alert editor dropdown |
| **Mode** | Sequential: one alert finishes before the next starts. Stack: alerts overlap, each starting after a fixed gap. |
| **Delay** | Seconds between one alert ending and the next starting (sequential), or between start times (stack) |
| **Max items** | How many alerts can be pending at once — extras are dropped. `0` = no limit |

Alerts in the **Default** queue play one at a time with no gap by default. Create additional queues to run separate simultaneous streams of alerts — for example a "main" queue for big events and an "ambient" queue for follows and likes that runs in parallel.

---

## Requirements

- Windows 10 or 11
- [plagComms](https://plagrizr.com) (provides the event stream)
- OBS Studio

---

## Part of the plagrizr Streaming Toolkit

plagCue is designed to work with **plagComms**, plagrizr's free multi-platform chat overlay app.

| App | What it does |
|---|---|
| **plagComms** | Connects to Twitch, TikTok, and YouTube. Displays a unified chat overlay in OBS. Provides the event stream that plagCue listens to. |
| **plagCue** | Receives events from plagComms and fires custom video/audio alert overlays in OBS. |

Both apps are free, self-contained, and run entirely on your machine.

---

## Keywords

stream alerts, OBS stream alerts, custom stream alerts, Twitch alert overlay, TikTok LIVE alerts, YouTube stream alerts, multi-platform stream alerts, OBS browser source alerts, sub alert, donation alert, raid alert, gift alert, channel point alert, free stream alerts, self-hosted stream alerts, no subscription stream alerts, stream overlay manager, Twitch TikTok YouTube alerts, plagrizr, plagCue, plagComms

---

## License & Credits

© 2026 plagrizr. All rights reserved.  
Provided free of charge for personal and community use.  
Not affiliated with Twitch, TikTok, or YouTube.

Built with Python 3.13, PyQt6, FastAPI, and uvicorn.
