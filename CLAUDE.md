# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A Dutch-language parental soundboard PWA ("Ouder Modus"). Parents record short audio clips (≤3 s) for each button, then tap them during situations with kids. Deployed to GitHub Pages at `https://agubernmerida.github.io/parental-soundboard/`.

No build step, no framework, no dependencies. Edit `index.html` directly.

## Deploying changes

```powershell
$env:PATH += ";C:\Program Files\Git\cmd"
cd C:\Users\Albert\Code\ParentalSoundboard
git add .
git commit -m "your message"
git push
```

GitHub Pages auto-deploys from `main` within ~1 minute.

## Architecture

Everything lives in `index.html`. The JS is a single-file state machine:

**Screen states** (controlled by `isAuthed()` + `mode`):
1. Gate — setup (first visit, no password set) or login (`psb-pwd-hash` exists in localStorage)
2. Play — fullscreen 3×3 grid; default after auth
3. Edit — card list for recording/managing buttons; reached via FAB

**`render()`** is the single entry point. It checks auth, then delegates to `renderGate()`, `renderPlay()`, or `renderEdit()`. All three write to `document.getElementById('app').innerHTML`. Event handlers are set as inline `onclick`/`oninput` attributes pointing to globals.

**Button data shape:** `{ id: string, emoji: string, label: string, audio: string|null }`  
Audio is stored as a base64 data URL (`data:audio/webm;...`). Entire `buttons` array is serialised to `localStorage` under key `parental-soundboard-v1`.

**Auth** uses Web Crypto SHA-256. localStorage keys: `psb-pwd-hash` (hex digest), `psb-authed` (`'1'`). Auth persists indefinitely per device — intentional, so the wife's phone stays unlocked.

**Recording flow:** `startRec(id)` → `getUserMedia` → `MediaRecorder` → chunks collected via `ondataavailable` → on `onstop`, blob → `FileReader.readAsDataURL` → stored in button. Auto-stops after `MAX_MS` (3000). Only one recording at a time (`recId` guard).

**Service worker** (`service-worker.js`): cache-first, precaches the four static files. Cache name is `ouder-modus-v1` — bump this string to force clients to update after a deploy.

## Key constraints

- **localStorage quota**: each 3 s clip is ~50–100 KB base64. 9 buttons ≈ under 1 MB — fine, but warn if adding many more.
- **Android Chrome only** for recording (MediaRecorder outputs `audio/webm`). Playback works on any browser.
- **`eh()`/`ea()`** are the HTML-escape helpers — use them whenever user-supplied content goes into innerHTML (`eh` for text nodes, `ea` for attribute values).
- The `.flash` animation requires the `void el.offsetWidth` reflow trick to restart on repeated taps.
