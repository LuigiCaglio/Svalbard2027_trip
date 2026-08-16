# Handoff — Svalbard 2027 Trip

For picking this back up later, in this repo or a new chat.

## Trip basics
- 3 people, Longyearbyen, Svalbard
- Apr 4, 2027 (arrive 13:15) – Apr 9, 2027 (depart 12:30)
- Same Longyearbyen hotel intended for all 5 nights

## Where everything lives
- Repo: https://github.com/LuigiCaglio/Svalbard2027_trip (public)
- Live app: https://luigicaglio.github.io/Svalbard2027_trip/ — Gantt-style schedule + interactive checklist, reads/writes `CHECKLIST.md` directly via the GitHub API
- Local clone: `C:\Users\luigi\OneDrive\Documenti\luigipersonal\my_apps\Svalbard2027_trip`
- `README.md` = itinerary reference. `CHECKLIST.md` = the actual checklist source (bookings/insurance/gear/phone/documents) — this is the live source of truth, check it (or the app) for current status rather than trusting this doc's snapshot.
- Editing on the live page needs a GitHub fine-grained personal access token (Contents: read/write, scoped to just this repo), pasted into "Unlock editing" and stored only in that browser's localStorage. Friends without GitHub accounts can view freely but not edit — they can ask Luigi to check things off for them, or make a free account + get added as a collaborator + generate their own token to self-edit.

## Finalized itinerary
| Day | Date | Plan |
|---|---|---|
| 1 | Apr 4 | Arrive 13:15, hotel, gear fitting, briefing |
| 2 | Apr 5 | Coal mine tour (morning) → 17:00 pickup for the 1-Night East Coast Adventure (snowmobile) |
| 3 | Apr 6 | East Coast trip day 2, back in Longyearbyen ~18:00 |
| 4 | Apr 7 | Ice Cave Hike – Frozen World, 09:30–~15:00 |
| 5 | Apr 8 | Free / not decided yet |
| 6 | Apr 9 | Pack, gear return, depart 12:30 |

## Key decisions & why (worth knowing before revisiting)
- **Pyramiden dropped** (both overnight stay and day trip): it's run by Trust Arktikugol, a Russian state company. Visit Svalbard delisted everything connected to it after Russia's invasion of Ukraine. The only operator still running trips there (GoArctica) is the Russian trust's own booking arm — decided it's not worth it, ethically or logistically.
- The originally-referenced "3-day East Coast Adventure" product (Svalbard Wildlife Expeditions) appears **discontinued** — their current live catalog only has the 1-Night and 2-Day versions (both are actually just 1 night at the cabin; "2-Day" only means a longer, earlier-starting first day). Went with the **1-Night** version.
- Considered Isfjord Radio and multi-day dog sledding as alternatives to the East Coast trip — East Coast won out as the strongest all-rounder for wildlife/wilderness on a first trip.
- For the shared checklist, the path taken was: Claude artifact (worked, but checkboxes were local-only per device, not synced) → GitHub Issue idea → considered a Firebase-backed page for true no-account sync → landed on the current setup: a public GitHub repo + a custom page that talks to the GitHub Contents API directly, so anyone can view for free and editing only needs a narrowly-scoped token, no separate backend service.

## Open items
- **Coal mine tour**: not booked yet, no operator chosen.
- **Apr 8**: still undecided — options discussed were leaving it free as a weather buffer, or filling it with another activity.
- Hotel, flights, insurance, SIM, and the full gear list: tracked as checkboxes in `CHECKLIST.md` / the live app — that's the current status, not this file.

## Useful facts surfaced during planning
- No aurora possible in April (too much daylight by then).
- EU roaming does **not** apply in Svalbard (outside the EU/EEA) — need a Telenor SIM or Telenor-network eSIM.
- No mobile signal out on the East Coast expedition — treat it as fully offline.
- East Coast trip: minimum group size 3 (matches the group exactly, no small-group supplement), requires a physical (non-digital) driving licence per person.
