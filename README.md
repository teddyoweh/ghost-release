# 👻 Ghost — Releases

Signed macOS builds of [**Ghost**](https://github.com/teddyoweh/ghost), a private,
screen-aware AI overlay. Source lives in the main repo; this one carries the
shipped binaries, checksums, and release notes.

**→ [Download the latest release](https://github.com/teddyoweh/ghost-release/releases/latest)**

| | |
| --- | --- |
| Latest | **v0.2.0** |
| Platform | macOS 11+ · **Apple Silicon only** |
| Signing | Developer ID Application · hardened runtime |
| Notarized | ❌ Not yet — see [First launch](#first-launch) |

---

## Install

1. Download `Ghost-0.2.0-arm64.dmg` from the [releases page](https://github.com/teddyoweh/ghost-release/releases/latest).
2. Open it and drag **Ghost** to Applications.
3. Launch it. Ghost is a menubar app — look for the dot in the menu bar, not the Dock.
4. Press `⌘\` to summon the overlay.

### First launch

The build is **signed but not yet notarized**, so Gatekeeper will refuse it the
first time with *"Ghost cannot be opened because Apple cannot check it for
malicious software."* This is expected and says nothing about the app — it means
the notarization ticket hasn't been issued yet.

To open it anyway:

- **Right-click** Ghost.app → **Open** → **Open** in the dialog, or
- **System Settings → Privacy & Security**, scroll to the bottom, click **Open Anyway**.

You only have to do this once per version.

You can confirm the build really is signed by you before trusting it:

```bash
codesign -dv --verbose=2 /Applications/Ghost.app 2>&1 | grep Authority
# Authority=Developer ID Application: Teddy Oweh (W5M7LV7263)
```

### Verify the download

```bash
shasum -a 256 -c SHA256SUMS.txt
```

Checksums for every release are in [`SHA256SUMS.txt`](SHA256SUMS.txt) and repeated
in the release notes.

---

## Permissions

Ghost asks for two macOS permissions, both on first use rather than at launch:

| Permission | Why | Where |
| --- | --- | --- |
| **Screen Recording** | Reading your screen for vision answers | Privacy & Security → Screen Recording |
| **Microphone** | Transcribing your side of a conversation | Privacy & Security → Microphone |

macOS only applies a *newly granted* Screen Recording permission after a relaunch.
Ghost detects this and offers a **Restart** button rather than failing silently.

Screen Recording is also what captures the **other** person's audio (system-audio
loopback), so live co-pilot needs it even when you're not using screenshots.

---

## Setup

Ghost needs at least one provider key. Everything is stored locally and encrypted
at rest with Electron's `safeStorage` — nothing is sent anywhere except the
provider you choose.

### Claude — API key (recommended)

Settings ⚙ → **Provider: Claude** → **API key**, paste `sk-ant-…`.

This unlocks the fast path: Ghost streams from the Messages API directly, with
prompt caching, effort control, and fast mode. Noticeably quicker per answer than
the subscription path.

### Claude — subscription

```bash
claude setup-token
```

Settings ⚙ → **Provider: Claude** → **Subscription**, paste the token. Routes
through the Claude Agent SDK, which also provides Exa web search. Correct, but it
forks a CLI subprocess per question, so it's slower.

### OpenAI

Settings ⚙ → **Provider: OpenAI**, paste `sk-…`.

An OpenAI key is **also required for live voice transcription**, regardless of
which provider answers — Ghost uses the OpenAI Realtime API for it. Removing that
dependency is on the roadmap.

### Give it your background

Settings ⚙ → **Your background**. Paste your résumé, the role you're targeting,
your projects and go-to stories.

This is the single biggest quality difference in the app. Without it you get
generic advice; with it, answers are built from work you actually did. It stays on
your machine and sits in the cached prompt prefix, so re-sending it every turn
costs almost nothing.

---

## Shortcuts

| Shortcut | Action |
| --- | --- |
| `⌘\` | Show / hide the overlay |
| `⌘⇧↩` | Capture the screen and ask |
| `⌘⇧G` | Live guidance from the conversation |
| `⌘⇧A` | Answer their most recent question specifically |
| `⌃⇧` + `← ↑ → ↓` | Move the overlay |
| `⌃⇧C` | Re-center the overlay |
| `↩` | Send · `⌘↩` sends **with** a screenshot |
| `esc` | Hide |

The first two are rebindable in Settings ⚙ → Shortcuts.

---

## Privacy

- Keys are encrypted at rest with Electron `safeStorage` and never leave the
  machine except as auth headers to the provider you picked.
- Screenshots go to your chosen provider and nowhere else. There is no Ghost
  server, no telemetry, no analytics.
- Session logs are plain markdown in `~/Documents/ghost-workspace/sessions/`,
  written locally. Toggle them off in Settings.
- `setContentProtection` keeps the overlay out of screen shares and recordings.
  It's on by default; turning it off means people on your call can see it.

---

## Known issues

- **Not notarized** — Gatekeeper warns on first launch. See [First launch](#first-launch).
- **Apple Silicon only.** No Intel build: the Claude Agent SDK ships a per-architecture
  binary and a universal build would break the subscription auth path.
- **Voice needs an OpenAI key** even if Claude is your provider.
- **Echo bleed on speakers.** "Them" is system audio and "Me" is the mic, so on
  laptop speakers their voice leaks into your side of the transcript. Echo
  cancellation helps; headphones help more.

---

## Links

- **Source** — [teddyoweh/ghost](https://github.com/teddyoweh/ghost)
- **Changelog** — [CHANGELOG.md](CHANGELOG.md)
- **All releases** — [releases](https://github.com/teddyoweh/ghost-release/releases)
