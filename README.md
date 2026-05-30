# Ouder Modus — Parental Soundboard

A simple, no-frills Progressive Web App (PWA) for parents. Record short audio clips (up to 3 seconds each) and tap them during challenging moments with kids. Fully offline-capable, no build step, no external dependencies.

**Live demo:** https://agubernmerida.github.io/parental-soundboard/

## Features

- 🎙️ **Record directly from your phone** — Android Chrome and iOS Safari (iPhone)
- 📱 **Installable on home screen** — works offline thanks to service worker
- 🎨 **Dutch language** with customizable buttons (emoji + label)
- 💾 **Per-device storage** — each phone keeps its own recordings
- 🔄 **Restore defaults** — reset all buttons or individual ones with one tap
- 📦 **No build, no dependencies** — edit `index.html` directly
- 🔀 **Versioned releases** — track updates with semantic versioning

## Usage

### For parents

1. **Visit** https://agubernmerida.github.io/parental-soundboard/
2. **Install** on home screen (+ button in Safari on iOS, menu → "Add to Home Screen" on Android)
3. **Record** by tapping ✏️ Bewerken, then 🎙 for each button (max 3 seconds)
4. **Play** by tapping buttons in the main grid
5. **Customize** emoji and labels in edit mode

### For developers

Clone the repo and edit `index.html` directly. No build step needed.

```bash
git clone https://github.com/agubernmerida/parental-soundboard.git
cd parental-soundboard
```

**Local testing:** Use a local HTTP server (not `file://`):
```bash
python3 -m http.server 8000
# Then visit http://localhost:8000
```

## Architecture

**Single-file app.** Everything is in `index.html`:

- **Render engine:** State machine with three screens (Play, Edit)
- **Storage:** localStorage for button recordings (base64 audio data URL)
- **Recording:** `getUserMedia` → `MediaRecorder` → `FileReader` → base64 → localStorage
- **Service Worker:** Cache-first strategy, precaches all assets for offline play
- **Default audio:** Loaded from `sounds/defaults.json` on first visit

**Button data shape:**
```js
{
  id: "1",
  emoji: "🛑",
  label: "Stop",
  audio: "data:audio/webm;codecs=opus;base64,GkXfo5..."
}
```

## Deployment

1. **Bump version** in `index.html`:
   ```js
   const VERSION = '1.1.0';  // → '1.2.0' for new features, '1.1.1' for fixes
   ```

2. **Bump service worker cache** (only if you changed cached assets):
   ```js
   const CACHE = 'ouder-modus-v6';  // → 'ouder-modus-v7'
   ```

3. **Push to GitHub:**
   ```bash
   git add .
   git commit -m "Add feature description"
   git push
   ```

**GitHub Pages auto-deploys** from `main` within ~1 minute.

## Browser Support

| Feature | Android Chrome | iOS Safari |
|---------|---|---|
| Play audio | ✅ | ✅ |
| Record audio | ✅ | ✅ |
| Offline | ✅ | ✅ |
| Install PWA | ✅ | ✅ |
| Default audio load | ✅ | ✅ |

## Storage Limits

- Each 3-second audio clip ≈ 50–100 KB (base64)
- 9 buttons ≈ < 1 MB total
- Safe on all modern browsers (localStorage quota is typically 5–10 MB per origin)

## File Structure

```
parental-soundboard/
├── index.html              # Main app (HTML + CSS + JS)
├── manifest.json           # PWA manifest
├── service-worker.js       # Offline cache strategy
├── icon.svg                # PWA icon
├── sounds/
│   └── defaults.json       # Default recordings (optional)
├── CLAUDE.md               # Development notes for Claude Code
└── README.md               # This file
```

## License

Open source. Use freely.

---

**Questions?** Check `CLAUDE.md` for detailed architecture notes.
