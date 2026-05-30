# Changelog

All notable changes to Ouder Modus are documented here.

## [1.2.0] — 2026-05-30

### Added
- **About section** with contact information (GitHub + email)
- **About modal** accessible from edit screen footer
- Contact info in README
- Author attribution in the app

### Changed
- Service worker cache bumped to v8

---

## [1.1.1] — 2026-05-30

### Fixed
- **Restore all** now re-adds deleted buttons (not just their audio)
- Deleted buttons can be recovered when restoring to defaults

### Changed
- Service worker cache bumped to v7

---

## [1.1.0] — 2026-05-30

### Added
- **App versioning system** — semantic versioning (MAJOR.MINOR.PATCH)
- **VERSION constant** in app (currently 1.2.0)
- **Version label** visible in edit screen footer
- **Deployment checklist** in CLAUDE.md
- README.md with comprehensive documentation

### Changed
- Removed Supabase cloud sync entirely
- Each device is now fully independent (no cross-device interference)
- Service worker cache strategy remains cache-first
- Service worker cache bumped to v6

### Why
- Users were experiencing unintended cross-device button deletions
- Supabase sync wasn't adding value for a family/personal app
- Local storage is sufficient for per-device recordings

### Architecture Impact
- No more cloud dependencies
- Faster deployments (no cloud sync delays)
- Each phone has complete autonomy
- Default audio still loads from `sounds/defaults.json` on first visit

---

## [1.0.0] — Initial Release

### Features
- 🎙️ Record audio directly from phone (Android Chrome, iOS Safari)
- 📱 Installable as PWA on home screen
- 🎨 Customizable buttons (emoji + Dutch labels)
- 💾 Local storage persistence (per device)
- 🔄 Restore individual buttons or all to defaults
- 📤 Export recorded audio as defaults.json
- ⚙️ Edit mode with card-based UI
- 🎯 Play mode with 3×3 button grid
- 📦 No build step, no dependencies
- 🔒 Fully offline-capable (service worker)

### Defaults
- 9 pre-configured buttons in Dutch
- Stop, Niet doen, Ga zitten, Eten, Slapen, Niet zeuren, Doe normaal, Niet met eten spelen, Geen vieze woorden zeggen
- Optional recorded audio for each button (loaded from defaults.json)

### Browser Support
- ✅ Android Chrome (recording + playback)
- ✅ iOS Safari (recording + playback)
- ✅ Offline playback
- ✅ PWA installation

---

## Development Notes

### Key Files
- `index.html` — Single-file app (HTML + CSS + JS)
- `manifest.json` — PWA manifest
- `service-worker.js` — Offline cache strategy
- `icon.svg` — App icon (512×512)
- `sounds/defaults.json` — Default recordings (base64 audio)
- `CLAUDE.md` — Architecture & deployment guide
- `README.md` — User documentation

### Versioning Strategy
- **MAJOR:** Breaking changes (unlikely for this app)
- **MINOR:** New features (e.g., new button management features, UI improvements)
- **PATCH:** Bug fixes (e.g., restore function fixes)

### Deployment Process
1. Bump `VERSION` in `index.html`
2. If cached files changed, bump SW cache name in `service-worker.js`
3. Commit and push to `main`
4. GitHub Pages auto-deploys within ~1 minute

### Local Testing
```bash
python3 -m http.server 8000
# Visit http://localhost:8000
```

### Known Limitations
- localStorage quota: ~5–10 MB per origin (9 buttons = ~1 MB, safe)
- Audio clips: max 3 seconds (enforced by MAX_MS)
- No authentication (intentional — each device independent)
- Recording only works on browsers with MediaRecorder support

---

## Future Ideas
- Multi-language support (currently Dutch with customizable buttons)
- Cloud backup option (opt-in, unlike Supabase sync)
- Dark mode
- Custom button grid sizes (currently 3×3 fixed)
- Sound themes / button packs
- Timer/reminder features

---

Generated: 2026-05-30  
Maintained by: Albert  
GitHub: https://github.com/agubernmerida/parental-dashboard
