# ROARRunlimited — interaction and expressive design

**Creative author:** Babuni.dev.

## The action bar as a rhythmic instrument

The creator's original tagline—**“ROARRunlimited turns your action bar into a war drum.”**—nicely captures the interaction. Instead of one special button making the character roar, different actions can carry their own intermittent gestures. An intense ability might roar frequently; another might occasionally cheer or flex.

This is not audio sequencer functionality: the “war drum” is a metaphor for expressive timing. The output is a built-in WoW emote chosen from four tokens.

## A number means two different things

The key UX distinction is between a **configuration instance** and a **physical action slot**:

| Setting | Interpretation |
| --- | --- |
| `/roed slot1 13` | Numbered instance **1** watches physical action-bar slot **13** |
| `/roed chance1 40` | Instance **1** gets a 40% chance |
| `/roed timer1 6` | Instance **1** gets a six-second cooldown |
| `/roed slot2 24` | Independent instance **2** watches physical slot **24** |

The suffix is a label chosen by the player, not a limit or the action slot itself. The code accepts many instance numbers; there is no “slot3 button” prerequisite.

`/roed watch` prints pressed slot IDs, making the system learnable without a visual UI. `/roed info` prints configured instance settings. `/roed reset` removes all instances at once; no current command deletes one particular numbered instance.

## Timing and duplication

Each instance runs its own elapsed-time gate. **The timestamp is advanced before the probability roll**, so an unsuccessful roll uses the cooldown too. The selected token is random after a successful chance roll. Two configurations targeting the same physical action slot may produce more than one emote from one press; no deduplication is implemented.

The addon can be globally silenced with `/roed off` without losing its configurations, but no in-game category controls or per-instance on/off flags are provided.

## Practical usability checks

- Does debug mode report understandable numeric slots across custom bar layouts?
- Are `slotX` / `chanceX` / `timerX` commands easy to remember?
- Does the global reset feel too destructive compared with a single-instance removal option?
- How does the addon behave if many instances are configured or one slot is registered multiple times?
- Are repeated in-world emotes appropriate for group play and accessible to players using localized game clients?

These are design verification questions, **not approved redesign requirements**.
