# Changelog

All notable changes to [Ghost](https://github.com/teddyoweh/ghost).

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [0.7.1] — 2026-10-08

### ✨ Added

- **Type into Ghost without leaving your app (Mac).** Press ⌃⌥Space or
  click **Keyboard** beside AGI, wait for the amber "Keyboard → Ghost"
  badge, and keys go to Ghost's composer while the other app stays in
  front. Enter sends, Esc exits. It turns itself off after 60 seconds, on
  an app switch, or under macOS Secure Input. Needs Accessibility / Input
  Monitoring. Basic text only: no IME or dictation, and not for passwords.

### 🔄 Changed

- **Showing Ghost no longer takes focus.** ⌘\ and the other shortcuts bring
  the overlay up without pulling the keyboard from the app you're in.

---

## [0.7.0] — 2026-10-06

### ✨ Added

- **Co-pilot shows the questions it hears.** When the other person asks
  something, the question pops up as a card with its answer already written
  behind it. *Show answer* (or ⌘⇧A) puts it on screen instantly.
- **AGI answers on its own** and is now the only mode that does. It can be
  switched on or off mid-call.
- **Typed questions know what was said.** Anything you ask during a call
  carries the conversation, so "answer that" works. AGI's screen answers
  hear the conversation too.
- **Drag the overlay with the mouse.** Grab the pill or any empty part of
  the card.

### 🐛 Fixed

- **Interviewer questions lost while you were talking.** When both sides
  spoke, the other person's words were dropped, and a sentence was cut to
  fragments whenever speech passed between the mic and the call. Their
  speech now takes priority, and nothing in progress is thrown away.
- **Co-pilot silently answering nothing.** Turning AGI off switched
  "Co-pilot answers" to manual behind your back, for every session after.
  The setting is gone; the Co-pilot and AGI buttons decide.
- **Question detection** now catches the problem being set, hints ("you
  might want to swap that less than") and "what questions do you have for
  me". It no longer fires on your own answers or halfway through an
  explanation.

---

## [0.6.3] — 2026-10-02

### ✨ Added

- **Sign in with ChatGPT from Settings.** Codex no longer expects a
  `codex login` run in a terminal. *Settings → Codex → Sign in with ChatGPT*
  opens the page, puts the one-time code on the clipboard, and the login
  lands on this computer by itself. Settings shows whether you're signed in,
  with Recheck and Sign out.

### 🐛 Fixed

- **"Reconnecting… 401 Unauthorized" on Codex.** That was Codex running
  with no login at all. Ghost now says it isn't signed in to ChatGPT yet and
  points to the button, instead of looping on the error.
- The startup notice and the overlay's sign-in banner ask for whichever
  provider is chosen (ChatGPT, OpenAI API key, or Claude), not always Claude.

---

## [0.6.2] — 2026-09-29

### ✨ Added

- **A new overlay.** A small control pill on top — Ghost, Hide/Show, Record —
  and a glass card under it: your question as a bubble, the answer, quick
  actions, and a composer with Co-pilot, Guide and AGI one click away. Hide
  folds it to just the pill.
- **Move it anywhere.** ⌘ + arrow keys while Ghost is focused, ⌃⌥ + arrow keys
  from anywhere (Ctrl and Ctrl+Alt on Windows).

### ⚡ Faster

- **Live answers land 1–2 seconds after a question ends** (was ~3.5). Claude
  Haiku decides when someone's asked something, in about half a second, and
  the answer starts writing while it decides. Ghost keeps its Claude
  processes warm during a session instead of starting one per answer.

### 🐛 Fixed

- **Small talk no longer triggers answers.** The on-device judge was
  throttled by macOS and fell back to a word count; Haiku reads the meaning.
- Session type (Live / Practice) moved into Settings.

---

## [0.6.1] — 2026-09-29

### ✨ Added

- **Screen recording.** *Settings → Save screen recording* keeps a video of
  your screen with both voices for each session, beside its audio. Ghost never
  appears in it. Off by default; up to about 700 MB an hour.
- **Notes that watch the recording.** Ghost picks the frames where the screen
  actually changed — plus the moments someone pointed at something — and
  writes notes from the transcript and the screen together, so the problem
  statement, the error and the test results make it in.
- **Screenshots and times in notes.** Key frames sit under the point they
  support and every topic carries the time it started; click either to play
  the recording from there.
- **Video in History.** The recording plays in the session's Recording card and
  stays pinned beside the notes, with the waveform, seeking and 0.5–2× speed.

---

## [0.6.0] — 2026-09-29

### ✨ Added

- **Session templates.** Save a brief for a kind of session — the format, the
  problem types, your own follow-ups — and Ghost reads it before the first
  question. Share one as a paste-able code or a `.ghosttemplate` file.
- **Live / Practice.** Tabs across the top of the overlay. Practice tells Ghost
  you're rehearsing a mock interview. The glass tints red for Live and green
  for Practice, so the mode is visible at a glance, even minimized.
- **Dashboard: Context, Templates, Hacks.** Who you are (folded into every
  session), your templates, and your ⌃⌥1–9 follow-ups beside every shortcut
  Ghost listens for. All of it autosaves.

### 🔧 Changed

- **Guide** looks before it answers and pastes code instead of typing it.
  **Drive** glides between moves and shows where it's about to act.

### 🐛 Fixed

- **Mac updates.** 0.5.3 and 0.5.4 were published without `latest-mac.yml`, so
  Macs stopped seeing updates. 0.6.0 carries the Mac and Windows feeds, and
  Macs on an older version pick it up at their next check.

---

## [0.4.1] — 2026-09-21

A one-fix follow-up to 0.4.0.

### 🐛 Fixed

- **The daily update check no longer leaves an error in Settings.** Ghost's
  update feed isn't publicly readable yet, so the background check fails — and
  because the updater reports failures whether or not anyone asked for one,
  0.4.0 parked a raw transport error in **Settings → About** for users who
  never pressed anything. Background checks now fail to the log and go back to
  idle; only a check you start reports, and it names the likely cause rather
  than quoting the updater.

---

## [0.4.0] — 2026-09-21

Ghost 0.3.1 worked on the machine it was built on. This release is what came
out of watching someone else try to run it.

### 🐛 Fixed

- **Listen worked on macOS 26 only.** The bundled `ghost-speech` helper was
  compiled for macOS 26 with a hard link to FoundationModels (the on-device
  "should I answer this?" judge), so on an older Mac dyld refused to load it and
  aborted the process at launch — taking live transcription down with it, even
  though Speech.framework itself has worked since 10.15. The helper now deploys
  to macOS 13 with FoundationModels weak-linked and every use of it behind an
  availability check; below macOS 26 the judge reports itself unavailable and
  Ghost falls back to its simpler trigger. The build fails if that regresses.

- **Three crashes that showed the unescapable "A JavaScript error occurred"
  dialog.** Starting a recording before `~/Documents/ghost-workspace/sessions`
  existed (`ENOENT`), audio still streaming into the speech helper after it
  died (`EPIPE`), and the sign-in helper's terminal closing mid-write. On a
  menubar app with no dock icon that dialog is close to impossible to get away
  from.

- **Quitting Ghost.** The menubar icon was an empty image — the tray item
  rendered nothing, and its menu was the only place "Quit Ghost" existed. There
  is a real icon now, plus **⌘Q** from the overlay or the dashboard and a
  **Quit Ghost** button in Settings. Quitting mid-recording also can't hang
  forever waiting for the audio to finish writing.

- **A fresh install refused every question** with *"No Claude OAuth token set.
  Run `claude setup-token`"* — even on a Mac where the `claude` CLI was signed
  in. Ghost now uses that login when it has no token of its own, and the same
  credential is used by live answers, the connection test, and History's notes.

- **Closing the welcome window** left Ghost running invisibly with a stranded
  dock icon. It now counts as skipping setup and shows the overlay.

- **Repeated identical errors.** When a credential was broken, every sentence
  spoken triggered another doomed answer and the overlay filled with red. Live
  guidance and screen autopilot now stop after three consecutive failures and
  say why once; saving Settings resumes them.

- **Meeting notes failed silently** in the dashboard — the button spun, nothing
  appeared, no explanation. The reason is shown now.

- **Misleading advice** when call audio wasn't captured: it told people to pick
  a screen in a share picker Ghost never shows. It names the real cause —
  Screen Recording permission — instead.

### ✨ Added

- **In-app updates.** Ghost checks the signed releases once a day and from
  **Settings → About**, downloads in the background, and installs only when you
  press *Restart & update* — nothing restarts itself while you're in a call. The
  menubar menu shows the same state. Running from the mounted disk image is
  detected and explained, since macOS makes that path unupdatable.

### 📋 Requirements

- **macOS 13 or newer** is now stated explicitly (it was always the real floor —
  system-audio capture needs it).
- **Drag Ghost to Applications** before launching. Run from the disk image,
  macOS re-randomizes its path on every launch, so screen and microphone
  permissions never stick and updates can't install.

---

## [0.3.1] — 2026-09-21

A distribution fix. No application code changed — this is the 0.3.0 build,
notarized. If 0.3.0 installed and ran for you, there is nothing new here.

### 🔒 Security

- **Builds are now notarized by Apple and stapled.** 0.3.0 was signed with a
  Developer ID but never submitted to Apple's notary service, so Gatekeeper
  refused it on every machine except the one that built it — downloaders were
  told macOS *"cannot verify this app is free of malware"* and had to
  right-click → **Open** to get past it. That workaround is gone: 0.3.1 opens
  with a normal double-click.

  Both the app and the disk image now carry their own notarization ticket, so
  verification also works offline:

  ```
  spctl -a -t exec /Applications/Ghost.app   # → accepted, Notarized Developer ID
  ```

  Teaching users to bypass Gatekeeper is bad practice for an app that asks for
  Screen Recording, Microphone, and Accessibility on first launch — the right-click
  habit is exactly what malware distribution relies on.

### 🔧 Build

- `npm run package:mac` now notarizes and staples by default, rather than only
  signing. Notarizing the app alone is not enough: the `.dmg` around it is a
  separate signable object needing its own ticket, which `scripts/notarize-dmg.sh`
  now handles.
- The packaging script fails loudly if the notary step was skipped. electron-builder
  silently downgrades a missing credential to a warning and still exits 0, which is
  how an unnotarized 0.3.0 shipped without anyone noticing.

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
