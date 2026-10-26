# ROARRunlimited — changelog and evidenced development

**Creator:** Babuni.dev. Historical dates below refer to Git commits, not asserted public release dates.

## 2026-10-26 — full documentation pass

- Expanded the project README, concept, roadmap, architecture, design/content register, decisions, narrative development journal and human–AI collaboration rules.
- Reconstructed the transformation from slot-indexed entries to numbered instances and the later 1.1 “FIXED” revision.
- Identified a **TOC/Lua filename capitalization mismatch**, saved cooldown timestamp risks and lazy-initialization logout risk without modifying runtime code.
- Documented four current emote tokens and per-instance chance/cooldown.
- **Markdown-only commit:** Lua, TOC and MIT license unchanged. In-game behavior not verified.

## Dated Git milestones

| Date | Commit | Observable change |
| --- | --- | --- |
| **2025-12-14** | `a02cbaf`, `d2c7d3c` | Original TOC text created then renamed to `ROARRunlimited.toc`. The TOC refers to lowercase `roarrunlimited.lua`. |
| **2025-12-14** | `47ffc04` | First `ROARRunlimited.lua` v1.0 uploaded, originally with **five** emote tokens, `ROEDDB`, and per-slot settings. |
| **2025-12-14** | `ad7d6a2` | Replaced non-Lua-5.0 `string.match` calls with `string.find`; moved chance and timer configuration from all watched slots toward particular configured slots. |
| **2025-12-14** | `58b9367` | Repaired the `PLAYER_LOGOUT` handler after a malformed intermediate code ending. |
| **2025-12-14** | `fcba0bf` | Source labeled v1.1; restructured configuration so numbered **instances** point to action-bar slots, instead of the physical slot ID being the table key. |
| **2026-01-09** | `ce86437` | Source header changed to “v1.1 FIXED”; removed BORED from the pool, normalized saved defaults and reset `last` on `slotX` assignment. |
| **2026-01-09** | `5919dcc` | Manifest version aligned to 1.1 and description updated. |
| **2026-09-18** | `ad85459` | Made README/source client wording provider-neutral (“Vanilla WoW 1.12”). |
| **2026-10-26** | `3a5fa29`, `6ba50e1`, `78eae98`, `0587573`, `eb2bcc2` | Author-credit and initial documentation scaffolding work before this expansion. |

The term **“FIXED”** is a source label, not evidence that packaging and runtime behavior were validated. The available Git history documents changes, not every original conversation or motivation.
