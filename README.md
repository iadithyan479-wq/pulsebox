# Pulsebox

> Music, in your hands.

Pulsebox is an open-source, local-first music streaming web app. It gives you a polished player for your own audio library and direct streaming URLs while keeping the core experience dependency-free and easy to self-host.

## What it is

Pulsebox is a **client-side music player**, not a catalog or a hosted music service. It does not bundle copyrighted recordings, scrape platforms, manage user accounts, or proxy audio. You bring audio from sources you are allowed to access:

- Local audio files from your device
- Direct audio URLs you control or are licensed to stream
- The metadata-only demo library included for exploring the interface

## Features

- Play, pause, previous, next, shuffle, repeat, seek, and volume controls
- Local audio file import with the browser File API
- Direct audio URL support
- Synced lyrics display with clickable timestamp navigation
- Web Audio frequency visualizer with a toggle in the player
- Expanded now-playing sheet with animated artwork and dynamic cover color
- Frosted-glass settings panel with reduced-blur accessibility mode
- Playback speed control from 0.5× to 2×
- Sleep timer presets and end-of-track stopping
- Save-audio action and optional player stats
- Search by title, artist, and album
- Queue management and recently played tracks
- Liked tracks stored for the current session
- Lightweight playlist creation
- Responsive dark interface with keyboard-friendly controls
- No backend, account, tracker, database, or API key
- Zero runtime dependencies and MIT license

## Quick start

### Prerequisites

- A modern browser with JavaScript enabled
- Python 3, Node.js, PHP, Ruby, or another static HTTP server
- Git if you want to clone the repository

### Run locally

```bash
git clone https://github.com/iadithyan479-wq/pulsebox.git
cd pulsebox
python3 -m http.server 8000
```

Open [http://localhost:8000](http://localhost:8000).

Node.js users can also run:

```bash
npx serve .
```

Pulsebox has no npm dependencies. `serve` is only a temporary static server.

## Using Pulsebox

### Play a local file

1. Click **Add local files** from the home or library view.
2. Select one or more audio files.
3. Open **Your library** to see them.
4. Click a track or use the player controls to start playback.

Local files are represented with temporary browser object URLs. They are not uploaded, and they disappear when the page is refreshed.

### Add a streaming URL

1. Click **＋ URL** in the top-right corner.
2. Paste a direct audio URL ending in a browser-playable format such as MP3, OGG, WAV, or M4A.
3. Add a title and artist.
4. Click **Add to library**.

The remote server must allow browser playback and the URL must point directly to audio. A normal YouTube, Spotify, SoundCloud, or web-page URL is not a direct audio stream and will not work as-is.

Only add and stream audio you have permission to access. Pulsebox does not bypass DRM, authentication, paywalls, or provider restrictions.

### Display lyrics

Click the `♫` button in the player to open the lyrics view for the current track. Timed lines are highlighted as the audio plays, and clicking a line seeks the track to that timestamp.

Lyrics use a small LRC-compatible format:

```text
[00:12.00] First line of lyrics
[00:18.50] Second line of lyrics
[01:02.25] A later line
```

When adding a streaming URL, paste the optional LRC text into the **Lyrics** field. Demo tracks include original sample lyrics; local files without metadata show an empty lyrics state.

### Use the visualizer

The `✦` button toggles a compact live frequency visualizer above the player. It uses the browser’s Web Audio API `AnalyserNode`, so the waveform is generated locally from the currently playing audio. Some browsers require the first click on Play before an audio context can start.

### Open the expanded player

Click the current artwork in the bottom player to open the expanded now-playing sheet. It includes animated artwork, dynamic cover color, lyrics, a save-audio action, a seek bar, and optional playback metadata.

Open **⚙ Settings** from the sidebar or player to configure:

- Animated artwork
- Dynamic color accents
- Player stats visibility
- Reduced glass blur
- Playback speed from 0.5× to 2×
- Sleep timers for 15, 30, or 60 minutes, or the end of the current track

### Search and navigate

Use the search field to filter tracks by title, artist, or album. The sidebar includes:

- **Home** — mixes, featured tracks, and recent activity
- **Your library** — every track loaded in the current session
- **Queue** — the upcoming play order
- **Liked tracks** — tracks liked during the session
- **Recently played** — tracks played in the current session
- **Playlists** — lightweight session playlists

### Player controls

- `▶` / `Ⅱ` — play or pause
- `↶` — previous track; restart the current track if more than four seconds have played
- `↷` — next track
- `⤨` — toggle shuffle
- `↻` — toggle repeat-current-track
- `♫` — open synced lyrics for the current track
- `✦` — toggle the audio visualizer
- `⚙` — open player settings
- Artwork thumbnail — open expanded now playing
- Progress slider — seek within the current audio
- Volume slider — adjust playback volume
- Heart — like or unlike the current track

## Demo library and licensing

The demo library contains metadata plus remote sample URLs so the interface can be explored quickly. Those URLs are not bundled music assets. For a production deployment, replace the demo catalog in `app.js` with audio you own or audio distributed under a license that permits your intended use.

Pulsebox is designed to keep the player separate from the catalog. A future adapter could load a catalog from a self-hosted JSON endpoint, a personal NAS, or a licensed music API without changing the core controls.

## Privacy and limitations

Pulsebox is local-first:

- Local files are read by the browser and played through temporary object URLs.
- No library, playlist, or listening history is sent to a Pulsebox server.
- There is no analytics, account system, backend, or tracking code.
- Session state is intentionally reset on refresh.
- The page references Google Fonts by default; remove those links from `index.html` for a fully offline UI.

Browser media policies may require a user click before audio playback begins. Cross-origin restrictions are controlled by the audio host; Pulsebox cannot make a remote server permit playback.

The visualizer receives detailed frequency data from local files and remote streams that opt into browser CORS. A remote stream without CORS headers can still play normally, but its visualizer data may be unavailable to the browser for security reasons.

## Project structure

```text
pulsebox/
├── index.html   # App shell, player controls, dialogs, and navigation
├── styles.css   # Dark responsive visual system
├── app.js       # Catalog, playback state, queue, playlists, and imports
├── README.md    # Documentation
└── LICENSE      # MIT license
```

The app deliberately uses browser-native APIs:

- `HTMLAudioElement` for playback and seeking
- `AudioContext`, `MediaElementAudioSourceNode`, and `AnalyserNode` for visualization
- `FileReader` and object URLs for local audio
- `URL.createObjectURL` for temporary downloads and local media
- Native `<dialog>` for adding streaming URLs
- In-memory JavaScript state for the session library and queue

## Development

No build step is required. After editing a file, refresh the page.

```bash
node --check app.js
python3 -m http.server 8000
```

When changing playback behavior, test:

- Local audio import with one and multiple files
- Direct URL addition with a browser-playable audio source
- LRC lyrics display, line highlighting, and timestamp seeking
- Visualizer toggle, audio-context startup, and responsive player layout
- Play, pause, seek, volume, and track navigation
- Shuffle and repeat behavior
- Search results and empty states
- Queue count and recently played state
- Small-screen responsive layout

## Roadmap

- [ ] Persist library, playlists, and preferences with IndexedDB
- [ ] Add drag-and-drop queue reordering
- [ ] Support album/artist views and richer metadata
- [ ] Add keyboard shortcuts and Media Session API integration
- [ ] Add a self-hosted JSON catalog adapter
- [ ] Add gapless playback and crossfade
- [ ] Add a service worker for the shell UI
- [ ] Add optional server-side authentication for private libraries

## Contributing

Contributions are welcome:

1. Fork the repository.
2. Create a focused branch, such as `feat/indexeddb-library`.
3. Keep the change small and document the user-facing behavior.
4. Run `node --check app.js` and test the app in a browser.
5. Open a pull request with screenshots for significant UI changes.

Please do not commit copyrighted recordings, private audio, credentials, API keys, or personal data. Keep demo content metadata-only or use clearly licensed assets.

## License

Pulsebox is released under the [MIT License](LICENSE).

## Android APK

Pulsebox includes a lightweight Android WebView wrapper under [`android/`](android/). It bundles the same local-first web app, so local files, direct streams, lyrics, visualizer, settings, and the expanded player are available in the APK.

### Install the latest APK

Download `Pulsebox-debug.apk` from the [latest GitHub Release](https://github.com/iadithyan479-wq/pulsebox/releases/latest), then open it on an Android device. Android may ask you to allow installation from your browser or file manager.

The debug APK is unsigned for production distribution but is installable for testing and personal use.

### Build locally

Requirements: JDK 17+, Android SDK Platform 35, and Android Build Tools 35.0.0.

```bash
cd android
./gradlew assembleDebug
# APK: app/build/outputs/apk/debug/app-debug.apk
```

The Android wrapper is intentionally small: [`MainActivity.java`](android/app/src/main/java/com/iadithyan/pulsebox/MainActivity.java) configures a WebView and loads the bundled assets from `app/src/main/assets/`.
