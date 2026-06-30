# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Japanese mahjong tournament manager (麻雀 対戦表メーカー＆点数入力) that generates fair seating schedules, tracks scores, and syncs data across devices via Firebase. It is a **zero-build vanilla JS single-page application** — no npm, no bundler, no TypeScript, no test suite.

## Running Locally

Open `index.html` directly in a browser, or serve with any static file server:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

Firebase credentials are hardcoded in `app.js` (lines 4–11). The app requires Google login and a room (Firebase room) before cloud features work. The schedule generator runs entirely client-side and does not need Firebase.

## Architecture

Three files total:

| File | Purpose |
|---|---|
| `index.html` | HTML skeleton; all interactive containers are empty `<div id="...">` injection points that JS fills |
| `app.js` | All ~1336 lines of logic: schedule generation, scoring, rendering, persistence, auth |
| `styles.css` | ~280 lines; all colours managed via CSS custom properties in `:root` |

### Core Data Model (`app.js`)

- **`current`** — the active tournament: `{id, P (players), G (target games), R (rounds played), T (tables/round), Bpr (byes/round), names[], rounds[]}`
- **`rounds[]`** — each element: `{tables: [[p0,p1,p2,p3], ...], byes: [p, ...]}` — values are **player indices (0-based)**, not names. Use `names[idx]` to resolve.
- **`scores`** — flat object keyed `"round_table_seat"` (e.g. `"0_1_2"`) → string point value

### Schedule Generation (`buildSchedule`, `addRound`)

The core algorithm uses **simulated annealing** to minimize repeated pairings:

1. Assign byes (weighted by fewest prior byes, avoiding consecutive byes)
2. Fisher-Yates shuffle remaining players into tables of 4
3. Simulated annealing: swap players between tables when it reduces `Σc²` (sum of squared pair-meeting counts)
4. Assign wind seats (East/South/West/North) by testing all 24 permutations per table across 5 passes to equalize seat distribution

`generate()` runs `buildSchedule` 12 times (or 6 for large tournaments) with different seeds and picks the best result by priority key: `[max-pairings, variance, seat-bias, -min-pairings]`.

`addRound()` extends an existing tournament by one round, seeding the annealing from the accumulated history (`deriveStats`).

The seeded RNG is Mulberry32 (`mulberry(seed)`): same seed → same schedule (reproducible).

### Score Calculation (`tableResult`)

Japanese scoring:素点 → 五捨六入 → ± ウマ → + オカ (1st place only).

- **Kaeshi (返し点)** = `kichi + oka/4` — the break-even raw score
- **Gosha-rokunyuu (五捨六入)** = round to nearest 1000: ≤500 rounds down, ≥600 rounds up (`gosha()`)
- **Auto-calculation**: if 3 of 4 seat scores are entered, the 4th is derived (`target - sum`)
- **Zero-sum correction**: after rounding, any ±1 discrepancy is absorbed into the least-constrained seat

Settings (`kichi`, `oka`, `uma`, `tie`, `tieScore`) live in the global `settings` object and are persisted in both localStorage and Firestore.

### Persistence

| Layer | What | Key/Path |
|---|---|---|
| `localStorage` | Active tournament (`current` + `settings`) | `LAST_KEY = "mahjong_last_v2"` |
| `localStorage` | Scores for each tournament | `"mahjong_scores_" + tournament.id` |
| `localStorage` | Last-used room ID | `ROOM_ID_KEY = "mahjong_room_id_v3"` |
| Firestore | Named tournaments (library) | `rooms/{roomId}/tournaments/{docId}` |

**Firestore limitation**: Firestore cannot store arrays-of-arrays. The full `rounds` structure is serialized to a JSON string in a single `payload` field; only flat metadata (`name`, `P`, `R`, `hasScores`, `savedAt`) is stored as top-level fields for list display.

### UI Patterns

- **Event delegation**: listeners are attached to stable parent elements (`$("rounds")`, `$("libList")`), never to dynamically-generated children. Individual items carry `data-*` attributes for identification.
- **Rendering**: `render()` rebuilds the entire rounds/tables HTML via template literals into `innerHTML`. `updateTotals()` is the lighter path — it updates only text nodes in already-rendered elements (called on every score keystroke).
- **XSS prevention**: all user-supplied strings go through `esc()` before `innerHTML` insertion.
- **Custom confirm/toast**: `uiConfirm()` and `uiToast()` replace `window.confirm/alert` (broken in some mobile WebViews).
- **`$` shorthand**: `const $ = id => document.getElementById(id)` — used everywhere.

### Auth and Room Model

Firebase Google Auth gates access. State globals: `currentUser`, `currentRoomId`, `currentRoomData`.

Room Firestore document structure:
```
rooms/{id}: { name, ownerId, inviteCode (8-char), memberUids[], members: { [uid]: {displayName, role, ...} } }
```

Startup sequence (bottom of `app.js`): `applySettingsToInputs()` → `restore() || generate(12345)` (schedule works offline) → `auth.onAuthStateChanged` loads cloud data once Firebase responds.
