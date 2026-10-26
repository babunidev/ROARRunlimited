# ROARRunlimited — development instructions

**Project creator and creative authority:** Babuni.dev.

## Preserve this project's role

ROARRunlimited is the **multi-instance** branch of the action-triggered emote idea: each numbered instance configures a physical action slot, chance and cooldown. Keep that architecture distinct from the single-slot `roar` and `roarr` repositories and from later RoarForever unless specific Git or creator evidence establishes a connection.

Read `README.md`, `CONCEPT.md`, `docs/ARCHITECTURE.md`, `docs/DEVLOG.md`, `docs/DECISIONS.md` and current Lua/TOC before designing any change.

## Collaboration history and truthfulness

1. Credit Babuni.dev for the idea and human creative direction; assign AI only work specifically evidenced as AI assistance.
2. Preserve source-backed iterations (2025 per-slot configuration → v1.1 instance numbering → 2026 four-token pool) without inventing dialogue, approval or rationale.
3. Separate user decisions from AI proposals and mark outstanding options clearly.
4. Every material change should update README, ROADMAP, CHANGELOG and relevant `docs/` records with exact changed paths and commit.
5. Never claim a successful WoW test when only inspecting source or Git diffs.
6. Preserve MIT license, copyright notice and author identification. The source's older display alias may be historically relevant; don't rewrite executable files in an incidental docs pass.
7. Do not publish credentials or private player conversations in portfolio narratives.

## Technical review checklist

- **Fix/verify case mismatch only with explicit implementation authorization:** `roarrunlimited.lua` in TOC versus `ROARRunlimited.lua` in tree.
- Use WoW 1.12/Lua 5.0-safe APIs. Historical source revisions replaced `string.match` with `string.find`.
- Preserve delegation to original `UseAction`; test global-hook interoperability and per-press iteration performance.
- A failed chance roll still consumes a per-instance interval.
- `cfg.last` is stored in persisted `ROEDDB.slots`; beware session-relative `GetTime()` after restarts.
- Loading of saved settings is lazy; `PLAYER_LOGOUT` saves whether or not `ensureLoaded` was invoked.
- Two instances can watch the same physical slot and each may emit one emote.
- Test actual client behavior and record failures as failures, not inferred successes.
