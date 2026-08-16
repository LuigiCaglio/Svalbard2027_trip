# Handoff — Svalbard 2027 Trip

For picking this back up later, in this repo or a new chat. This app grew a lot beyond a simple checklist — read the "How the app works" section before touching the code.

## Trip basics
- 3 people (Luigi, Andrea, Alessandro), Longyearbyen, Svalbard
- Apr 4, 2027 (outbound flight CPH→LYR departs 08:05, lands 13:15) – Apr 9, 2027 (depart Longyearbyen 12:30)
- **Only the flight is booked so far.** Everything else (hotel, mine tour, East Coast expedition, ice cave hike) is planned but not yet paid for — don't assume otherwise.

## Where everything lives
- Repo: https://github.com/LuigiCaglio/Svalbard2027_trip (public)
- Live app: **https://luigicaglio.github.io/Svalbard2027_trip/**
- Local clone: `C:\Users\luigi\OneDrive\Documenti\luigipersonal\my_apps\Svalbard2027_trip`
- Already sent to the friend group — they can view everything with zero account.

## How the app works
`index.html` is a single self-contained page hosted on GitHub Pages. It has **no backend of its own** — all "live" data is read and written directly through the GitHub Contents API (`api.github.com/repos/LuigiCaglio/Svalbard2027_trip/contents/...`), using `fetch()` calls already in the script (`ghGetRaw` / `ghPutRaw` / `ghDeleteRaw` helpers).

- **Viewing** needs nothing — public repo, public API reads work with no auth (subject to GitHub's low unauthenticated rate limit, fine for this traffic level).
- **Editing** (checkboxes, add-item forms, voice recording, hero photo upload, schedule edits) requires a GitHub fine-grained personal access token (Contents: read/write, scoped to just this repo). Click "Unlock editing" at the bottom of the page, paste the token — it's stored only in that browser's `localStorage` (key `svalbard-gh-pat`), never sent anywhere but directly to GitHub's API from the browser.
- Friends without a token can still view live data and can ask Luigi (or any collaborator) to make changes for them.
- To let a friend self-edit: GitHub Settings → Collaborators → invite them (needs a free GitHub account on their end) → they generate their own fine-grained token the same way Luigi did.

### Data files (all in repo root unless noted), each with a hardcoded JS fallback if the file doesn't exist yet
| File | Powers | Fallback if missing |
|---|---|---|
| `CHECKLIST.md` | Bottom checklist sections (Prenotazioni, Vestiario e attrezzatura, Documenti) | n/a, must exist |
| `schedule.json` | Gantt chart bars (Hotel/Miniera/Slitta malata/Grotta/Libero) — click a bar to view/edit day(s), time, notes | `DEFAULT_SCHEDULE_ROWS` in index.html |
| `dafare.json` | "Da fare" list, array of `{text, done}`, one checkbox each | `DEFAULT_DAFARE_ITEMS` |
| `alloggi.json` | "Possibili alloggi" list | 2 defaults (Radisson Blu, Coal Miners' Cabins) |
| `prenotato.json` | "Prenotato" list (confirmed bookings) | empty |
| `escursioni.json` | "Possibili escursioni" table (Nome/Link/Note) | 3 defaults (the decided-among options) |
| `hero.json` | `{"path": "assets/hero.jpg"}` — points to the banner photo | hero section stays hidden |
| `assets/hero.jpg` | The actual banner photo (AI-generated group photo, polar bear joke image) | — |

Alloggi/Prenotato/Escursioni all share one generic module (`makeLinkedListModule` in the script) since they're structurally identical (name/link/note + add form). Da fare and the checklist are separate, bespoke implementations.

### Other notable built features
- **Countdown**: full D/H/M/S at the top; once scrolled past, a small pill fixed to the top of the viewport shows just the day count (rebuilt this way specifically to fix a scroll-position feedback-loop bug — don't go back to a `position: sticky` + height-changing-content approach). Targets the actual CPH departure (08:05 Apr 4), not the Svalbard arrival.
- **"vicina" word**: styled in Playfair Display italic with wide letter-spacing. Clicking it plays the currently-selected friend's 3-second recorded voice clip (Luigi/Andrea/Alessandro, picked via the dropdown next to it). The 🗣️ icon reads the phrase prefix aloud via the browser's built-in Web Speech API (`it-IT`), then automatically chains into the recorded "vicina" clip when the TTS finishes — sounds like one sentence. Voice clips are recorded client-side via Web Audio API (not `MediaRecorder`, for cross-browser consistency) and saved as `audio/1.wav` / `2.wav` / `3.wav`.
- **Hero photo**: banner under the title, click to enlarge in a lightbox. Upload a new one via the file picker at the bottom (needs token).
- Theme is light/ice-toned only (no dark-mode-specific redesign beyond the basic `prefers-color-scheme` swap already in place).
- Everything user-facing is in Italian; code comments/variable names/commit messages are in English.

## Finalized itinerary (still just planned, not booked — see above)
| Day | Date | Plan |
|---|---|---|
| 1 | Apr 4 | Land 13:15 (flight departs CPH 08:05), hotel, gear fitting, briefing |
| 2 | Apr 5 | Coal mine tour, Gruve 3 (morning) → 17:00 pickup for the 1-Night East Coast Adventure (snowmobile) |
| 3 | Apr 6 | East Coast trip day 2, back in Longyearbyen ~18:00 |
| 4 | Apr 7 | Ice Cave Hike – Frozen World, 09:30–~15:00 |
| 5 | Apr 8 | Free / not decided yet |
| 6 | Apr 9 | Pack, gear return, depart 12:30 |

## Key decisions & why (worth knowing before revisiting)
- **Pyramiden dropped** (both overnight stay and day trip): it's run by Trust Arktikugol, a Russian state company. Visit Svalbard delisted everything connected to it after Russia's invasion of Ukraine. The only operator still running trips there (GoArctica) is the Russian trust's own booking arm — decided it's not worth it, ethically or logistically.
- The originally-referenced "3-day East Coast Adventure" product (Svalbard Wildlife Expeditions) appears **discontinued** — their current live catalog only has the 1-Night and 2-Day versions (both are actually just 1 night at the cabin; "2-Day" only means a longer, earlier-starting first day). Went with the **1-Night** version.
- Considered Isfjord Radio and multi-day dog sledding as alternatives to the East Coast trip — East Coast won out as the strongest all-rounder for wildlife/wilderness on a first trip. These alternatives were later removed from the visible "Possibili escursioni" table (git history has them) since the friend group only seriously discussed the 3 that remain listed.
- For the shared checklist, the path taken was: Claude artifact (worked, but checkboxes were local-only per device, not synced) → GitHub Issue idea → considered a Firebase-backed page for true no-account sync → landed on the current setup: a public GitHub repo + a custom page that talks to the GitHub Contents API directly, so anyone can view for free and editing only needs a narrowly-scoped token, no separate backend service.
- **Assicurazione and Telefono e connettività sections were removed from the visible checklist** (too much to read, per Luigi) — content still fully recoverable from git history (search log for "Assicurazione" / commit around "Trim checklist"), not deleted for good. Vestiario e attrezzatura was emptied out the same way, ready to be refilled.

## Open items
- **Coal mine tour**: not booked yet, no operator confirmed (link to Gruve 3 is in the Escursioni table).
- **Apr 8**: still undecided — options discussed were leaving it free as a weather buffer, or filling it with another activity.
- **Hotel**: not booked yet either.
- Nothing else is booked except the flight — see "Prenotato" list on the live page for current status (currently empty).

## Useful facts surfaced during planning
- No aurora possible in April (too much daylight by then).
- EU roaming does **not** apply in Svalbard (outside the EU/EEA) — need a Telenor SIM or Telenor-network eSIM.
- No mobile signal out on the East Coast expedition — treat it as fully offline.
- East Coast trip: minimum group size 3 (matches the group exactly, no small-group supplement), requires a physical (non-digital) driving licence per person.
