# ROARRunlimited — verification backlog and open proposals

**Author and decision-maker:** Babuni.dev  
**Status:** test planning and possible future choices, not approved implementation milestones.

## Current source baseline

- Version 1.1, TOC interface `11200`, one Lua source file and account-wide saved variables `ROEDDB`.
- Unlimited numbered **instances** in principle, each holding `slot`, `chance`, `cd`, `last`.
- Four native emotes: ROAR, CHARGE, CHEER, FLEX.
- Slash command `/roed`; `watch`, `slotX`, `chanceX`, `timerX`, `info`, `reset`, `on`, `off`.
- No automated test suite found. No client runtime tested in this review.

## Priority 0 — resolve packaging before gameplay claims

**Filename case mismatch:** `ROARRunlimited.toc` references `roarrunlimited.lua`, but the file is stored as `ROARRunlimited.lua`. Test loading on a case-sensitive installation or approve a minimal packaging correction before claiming the addon works on all supported environments. No code/TOC change was made here.

## Priority 1 — functional checks

1. Confirm TOC load, correct slash registration and absence of Lua errors on the target client.
2. Toggle `/roed watch`; identify slots, create `slot1` and `slot2` with different physical numbers, then query `info`.
3. Set distinct `chance1/chance2` and `timer1/timer2`; verify output is independent per instance.
4. Test `chance1 0` and `chance1 100`, and `timer1 0`; verify missed chance rolls still consume cooldown.
5. Map **two instances to one physical slot**; check whether a single press can produce two emotes and whether that is desired.
6. Check repeated `slotX` assignment: `last` is reset to 0 while existing chance/timer survive.
7. Test `/roed off`, `on`, `reset` and persistence through `/reload` and logout.
8. Test **fresh client restart** after a high saved `cfg.last`; compare to new `GetTime()` uptime to see whether the old timestamp suppresses emotes unexpectedly.
9. Test **login then logout without pressing an action or issuing a slash command**: lazy loading may allow `PLAYER_LOGOUT` to save empty/default state over saved `ROEDDB` values.
10. Test coexistence/load order with `roar`, `roarr`, and other addons replacing global `UseAction`; test all four native tokens on the chosen client.
11. Check resource impact of many configurations: `UseAction` loops through **all** instances on every button press.

For each test, record client/build, exact addon folder, loaded addons, steps, expected and observed output, error logs, and commit SHA.

## Open design/technical choices — not approved

- Correct filename capitalization in the TOC or rename the actual Lua file?
- Persist `cfg.last` or treat cooldown timestamps as session-local state?
- Permit duplicate-slot instances intentionally, or enforce one instance per physical slot?
- Keep per-instance numbers as the only UI or add an easier listing/renaming workflow?
- Is removing BORED a permanent creative direction?
- Should `/roed info` display the remaining cooldown, or is the current summary sufficient?
- Does Babuni.dev want this as a distinct portfolio artifact in a ROAR lineage, or a maintained replacement?

Documentation work does not select these options or authorize modifications.
