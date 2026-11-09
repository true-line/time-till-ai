# Time till' AI

Provider quota-window tracker for AI coding harnesses — by **True-Line AI LLC** · [timetillai.app](https://timetillai.app)

Tracks when each provider's quota resets: **5-hour rolling windows** (Claude, Codex, Z-Code, Muse…), **calendar quotas** (OpenCode weekly), and **token pools with cooldowns** (Qwen Code free tier). Static page + IndexedDB — **zero backend, zero cost**.

## Run it

Open `index.html` in any modern browser — that's the whole app. All data lives in that browser's IndexedDB.

## Deploy to timetillai.app

It's 4 static files (`index.html`, `manifest.webmanifest`, `icon.svg`, this README). Any static host works:

- **Cloudflare Pages** (recommended): create project → upload this folder → add custom domain `timetillai.app` (nameserver or CNAME at your registrar). Free, no functions needed.
- **GitHub Pages**: push to a repo → Settings → Pages → custom domain `timetillai.app`.

## Arming windows from your agent (no UI taps)

The app parses hash links on load — open them via `open` (macOS), `xdg-open` (Linux), `start` (Windows), or a browser:

```
https://timetillai.app/#start=claude              arm the 5h window now
https://timetillai.app/#start=claude&in=90m       custom length
https://timetillai.app/#cap=codex&resets=18:44    reset time from the CLI's limit message
https://timetillai.app/#cap=codex&in=38m
https://timetillai.app/#drain=qwen&used=620k      token pool exhausted → cooldown starts
```

`&src=<agent-name>` tags the origin. Links are idempotent — they never double-arm an active window.
The **Agent links** button in the app shows this table plus a copyable instruction block — paste it into any harness and the agent wires its own hooks.

## Sync across devices via gist

1. Create a **secret gist** on GitHub (any name), copy its ID from the URL.
2. In the app → cloud icon → paste the ID as the **session code**. Read works anywhere with just the code (even `curl`).
3. To push from this device, add a GitHub **fine-grained token** with gist write permission (stored locally only). State syncs as one JSON file, `timetillai-state.json`, last-write-wins by revision.

## Views

- **Board** — next-reset hero, depleting rings (visible ring = remaining capacity), rolling 24h timeline, recent windows.
- **Runway** — remaining-capacity bars per provider, sorted by soonest reset.
- **Terminal** — dense glance view; the tab title also carries the next countdown.

## Data notes

- IndexedDB stores: `providers`, `windows`, `kv` (settings). State is always derived from timestamps — closing the tab for a day loses nothing.
- Provider branding comes from the favicon service (unavatar.io) with a letter-mark fallback; accent color is canvas-sampled from the logo when CORS allows, else derived from the name.
- `fresh` decays to `ready` after 90 minutes to keep the board quiet.
- Export/Import JSON lives in Settings; gist sync is the cross-device path.

## Roadmap ideas

- Service worker for full offline + installable-with-badge PWA.
- MCP tool wrapper (`arm_window(provider)`) as an alternative to deep links.
- Auto-detect "resets at …" from clipboard/paste.

## License

MIT — see [LICENSE](LICENSE). © 2026 True-Line AI LLC.

## For agents

`llms.txt` and `llms-full.txt` are served at the site root for machine discovery — the full deep-link protocol, semantics and roster for automated integrators.
