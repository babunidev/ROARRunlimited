# ROARRunlimited — architecture, version 1.1

**Evidence:** `ROARRunlimited.lua` and `ROARRunlimited.toc` on 2026-10-26. Static analysis only.

## Data flow

```text
Player presses an action-bar button
    |
Wrapped global UseAction(slot, checkCursor, onSelf)
    |-- ensureLoaded(): bind account-wide ROEDDB.slots
    |-- WATCH_MODE? -> report physical action slot
    |-- for each configuration instance in WATCH_SLOTS:
    |      if cfg.slot matches pressed slot:
    |         doBattleEmoteForSlot(cfg)
    |           |-- ENABLED?
    |           |-- GetTime() - cfg.last >= cfg.cd ?
    |           |-- cfg.last = now   <-- before chance roll
    |           |-- random <= cfg.chance ?
    |           '-- DoEmote(random token) or fallback EMOTE text
    '-- forward original arguments to _Orig_UseAction
```

The ordinary action is delegated regardless of whether any emote is fired. The trigger checks the **button press**, not combat state or cast success.

## Configurations are numbered instances

```text
ROEDDB
  enabled = true / false
  slots = {
    [1] = { slot = 13, chance = 40, cd = 6, last = ... },
    [2] = { slot = 24, chance = 80, cd = 12, last = ... },
    ...
  }
```

`slot1 13` means “instance 1 watches physical slot 13”, not “watch physical slot 1”. `chance1` and `timer1` edit instance 1's settings. Slots and numbered instances are different concepts.

- New instance: default `chance = 100`, `cd = 6`, `last = 0`.
- `slotX` resets the instance's `last` timestamp to zero even when retargeting an existing instance; chance/cooldown remain otherwise unchanged.
- Loaded saved entries with nil fields are normalized by `ensureLoaded`.
- `/roed reset` clears every configuration instance. There is no per-instance delete command.
- `/roed on/off` toggles one global enabled flag, not per-instance switches.
- The code imposes no small fixed instance count. It scans **all instances on every action press**, so resource cost rises with the number of entries.

Two instances may watch one physical slot, so a single press can independently evaluate both (and potentially emit multiple emotes).

## Emote output

The source contains **four** `DoEmote` tokens: `ROAR`, `CHARGE`, `CHEER` and `FLEX`. The earlier BORED token was removed in January 2026. If `DoEmote` is unavailable, the fallback is `SendChatMessage("makes a battle cry!", "EMOTE")`. Exact animation/chat behavior depends on client support and has not been tested here.

## Persistence and lifecycle

The TOC declares account-wide `SavedVariables: ROEDDB`. `ensureLoaded()` only runs from the `UseAction` wrapper and `/roed` command handler. `PLAYER_LOGIN` seeds the global Lua random generator; `PLAYER_LOGOUT` writes `ROEDDB.slots` and `ROEDDB.enabled`.

**Risk A, session-relative clocks:** `cfg.last` is updated in a configuration table that is persisted. `GetTime()` reports client uptime, which changes across game sessions. Restoring a large previous `last` after a fresh start could make `now - cfg.last < cfg.cd` true for an unexpectedly long time. The source does not reset all loaded timestamps at login.

**Risk B, uninitialized logout:** logout writes local `WATCH_SLOTS = {}` and `ENABLED = true` if no slash command or `UseAction` call triggered `ensureLoaded`. This may overwrite previously saved settings. Both risks are source-derived, **not reproduced in a game client**.

**Risk C, global hook:** capturing and replacing `UseAction` can interact with other addon wrappers according to load order.

## Packaging and Lua compatibility

- Repository file: **`ROARRunlimited.lua`** (capitalized).
- TOC script reference: **`roarrunlimited.lua`** (lowercase).
- Case-sensitive environments may fail to load the Lua; resolve/verify before claiming successful deployment.
- Version 1.1, interface `11200`; slash command `/roed`.
- Command patterns use Lua 5.0-era `string.find` rather than the original v1.0 `string.match`.
- Numeric inputs are parsed with `tonumber`, without enforcing physical action-slot bounds or validating small integer-only indices.

No Lua, TOC, license, sound/media, GUI, or network-layer changes were made in the documentation pass.
