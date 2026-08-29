# Changelog

All notable changes to [Ghost](https://github.com/teddyoweh/ghost).

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [0.2.0] — 2026-08-28

First distributed build. A rewrite of the answer path around the thing that
actually decides whether Ghost is useful: how long it takes to get a readable
answer on screen during a live conversation.

### 🔒 Security

- **Removed a hardcoded Exa API key** that shipped as a config default in 0.1.0.
  It was embedded in plaintext in every build, so anyone with the app had it. It
  is now a required Settings field with no default. **If you ran a 0.1.0 build,
  that key is compromised — rotate it.**
- **Link handling in answers is now scheme-checked.** Answers are model output
  shaped by whatever is on your screen, which makes a link inside one untrusted
  input. Only `http`/`https` URLs reach the shell; a `file://` or `javascript:`
  target from a malicious page in a screenshot is now inert.

### ⚡ Performance

- **Screenshots are ~50× smaller.** Ghost was sending full-resolution retina PNGs
  (≈6.7 megapixels). Vision models downscale anything past ~1568px on the long
  edge before they ever look at it, so the extra pixels bought nothing but upload
  time. Captures now happen at that size, encoded as JPEG.
- **Direct Messages API path.** With an Anthropic API key and web search off,
  Ghost streams from the API directly instead of forking the Agent SDK's bundled
  CLI subprocess for every question. That removes a process spawn, a Node boot,
  and an auth handshake from the critical path of each answer.
- **Prompt caching** on the stable system prefix (persona + your background), with
  a 1-hour TTL.
- **Reasoning effort control**, defaulting to `low`. Live guidance always runs at
  `low` regardless of the setting.
- **Fast mode** on Claude Opus 5 and Opus 4.8 — up to 2.5× output tokens/sec at
  premium pricing. Off by default, toggleable in Settings.
- **Hot-frame reuse.** A screenshot taken within the last 1.5s is reused rather
  than re-grabbed, so a burst of live guidance doesn't re-capture the screen for
  each request.
- **The screen watcher stopped burning battery.** It was taking a
  full-resolution capture ten times a minute purely to average it down to a
  64-pixel perceptual hash. It captures 64×64 now.

### ✨ Added

- **Your background** (Settings ⚙). Résumé, target role, projects, go-to stories —
  folded into the cached prompt prefix. This is the single biggest quality lever in
  the app: it turns a generic STAR scaffold into an answer built from work you
  actually did. Never leaves the machine.
- **Teleprompter lead line.** Prompts now put the say-this-now sentence first, and
  the overlay renders that first paragraph at a size you can read in a two-second
  glance mid-conversation.
- **Session logs.** Every question, answer, and transcript line is appended to a
  markdown file in `~/Documents/ghost-workspace/sessions/`, so a call can be
  reviewed afterwards. Toggleable.
- **Live cost meter** in the overlay header — running spend and request count for
  the session, click to reset. Autopilot can fire a vision request on every
  question the other person asks, and that adds up quietly.
- **Signed `.dmg` and `.zip` builds**, plus an opt-in notarization target and CI
  that typechecks and builds on every push.

### 🐛 Fixed

- **The Stop button could silently stop working.** Starting a new request
  interrupted the previous one, whose teardown then ran *after* the new request
  had registered itself and cleared the new cancel handle. Requests are tracked by
  id now, and teardown only releases the slot if it still owns it.
- **A cancelled request could corrupt the answer you were reading.** Every stream
  event carries its request id and the UI routes by that id, instead of applying
  whatever arrives to whichever turn happens to be active.
- **Live guidance now re-asks when the question grows.** Instant mode fires on a
  partial utterance, but the finished question often carries the part that matters
  ("…in a distributed system?"). If the final transcript is materially longer than
  the partial that triggered the answer, Ghost asks again.
- **Cancelled requests no longer leave an empty turn** stuck in the transcript.
- Dropped a dead `gpt-4o` fallback that contradicted the configured OpenAI model.

### 🔄 Changed

- **Model list refreshed.** Claude Opus 5 was missing entirely; it's now the
  default. Opus 4.8, Sonnet 5, Haiku 4.5, and Fable 5 are selectable, each labelled
  with what it's good for. Retired IDs migrate forward automatically; a model you
  deliberately picked is left alone.
- **Ghost is a proper menubar app.** `LSUIElement` is set, so it no longer appears
  in the Dock or the ⌘-Tab switcher.
- Answers lead with the actionable line and put caveats and code after it, across
  all four prompt modes.

### 📦 Build

- `npm run package:mac` → signed `.dmg` + `.zip`, Apple Silicon.
- `npm run package:mac:notarized` → the above plus Apple notarization and stapling.
  Notarization is opt-in because it requires Apple credentials in the environment,
  and making it mandatory would mean no build at all without them.
- The disk image is signed too. electron-builder signs the `.app` but leaves the
  `.dmg` wrapper unsigned unless it's also notarizing.
- **Apple Silicon only, deliberately.** The Claude Agent SDK ships a
  per-architecture `claude` binary; a universal build would ship only one of them
  and break the subscription auth path at runtime.

### Known issues

- Not notarized — Gatekeeper warns on first launch. Right-click → Open, once.
- Live voice transcription requires an OpenAI key even when Claude is answering.
- On laptop speakers, the other person's voice bleeds into your side of the
  transcript. Headphones fix it.

---

## [0.1.0] — 2026-07-20

Initial build. Not distributed.

### Added

- Transparent, frameless, always-on-top overlay that follows you across Spaces and
  fullscreen apps.
- **Excluded from screen capture** via `setContentProtection` — invisible to
  Zoom, Meet, and QuickTime recording.
- On-demand screen capture as vision input.
- Claude (via the Claude Agent SDK, subscription or API key) and OpenAI providers.
- Live transcription of both sides of a conversation via the OpenAI Realtime API,
  with system-audio loopback for the other participant.
- Interview and meeting co-pilot modes with instant / balanced / manual guidance
  sensitivity.
- Autopilot: watches the screen and answers anything actionable.
- Exa web search through the Agent SDK.
- Streaming answers with markdown and syntax highlighting, rebindable global
  shortcuts, menubar tray, keys encrypted at rest with `safeStorage`.

[0.2.0]: https://github.com/teddyoweh/ghost-release/releases/tag/v0.2.0
[0.1.0]: https://github.com/teddyoweh/ghost/commit/b68999c
