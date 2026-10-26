# ROARRunlimited — turn your action bar into a war drum

**Creator / author:** Babuni.dev  
**Target:** World of Warcraft 1.12 / Lua 5.0, as declared in the addon  
**Version:** 1.1 (manifest and source)  
**Runtime status:** Not tested during this documentation review

ROARRunlimited builds on the action-triggered emote idea by letting a player associate **as many numbered configurations as needed** with action-bar slots. Each numbered instance has its own chosen action slot, trigger chance, and cooldown. Pressing a configured button can play a randomly selected native WoW battle emote while leaving the button's normal action intact.

The creator's original README captured the personality of the project in one line: **“ROARRunlimited turns your action bar into a war drum.”** This is character-flavor tooling, not a spell rotation, combat automation, or a custom audio/animation system.

## Important packaging check

**The current TOC names `roarrunlimited.lua`, but the repository's file is `ROARRunlimited.lua`.** These differ in capitalization. That may prevent loading on case-sensitive file systems or addon loaders. For a deployment where filename case matters, ensure the file reference and actual filename match. This documentation pass flags the inconsistency but **does not rename or edit runtime files**.

The intended folder layout, pending that correction or client verification, is:

```text
Interface/AddOns/ROARRunlimited/
  ROARRunlimited.toc
  ROARRunlimited.lua
```

Restart the game and enable the addon. Confirm that the game actually loads the Lua code before following the commands below.

## Quick setup

```text
/roed watch
```

Press the buttons you want to use and note their reported action-slot numbers. Type `/roed watch` again to turn off the debug messages.

```text
/roed slot1 13
/roed slot2 24
/roed chance1 40
/roed timer1 6
/roed chance2 80
/roed timer2 12
/roed info
```

This example configures instance **1** for action slot 13 (40% chance, six-second interval) and instance **2** for slot 24 (80%, twelve seconds). Instances are numbered labels; the digit after `slot`, `chance`, or `timer` identifies the **same configuration**, not the physical action-slot number.

### Commands

| Command | Source-defined behavior |
| --- | --- |
| `/roed watch` | Toggle reporting of pressed slot numbers |
| `/roed slotX <slot>` | Create/update numbered instance `X` to watch an action slot; reset that instance's last-attempt time |
| `/roed chanceX <0-100>` | Set chance for existing instance `X` |
| `/roed timerX <seconds>` | Set a nonnegative cooldown for existing instance `X` |
| `/roed info` | Show each instance's slot, chance and cooldown |
| `/roed on`, `/roed off` | Enable or disable triggering without clearing configurations |
| `/roed reset` | Clear **all** configured instances |
| `/roed` | Display a condensed command summary |

An instance must exist before its chance and timer can be changed. Defaults on creation are **100% chance** and **six seconds**. There is no GUI and no explicit maximum instance number in the parser, although large configurations would still use resources.

## What actually happens

On a regular action-bar press, the addon scans its numbered instances and checks those assigned to that slot. Each matching instance independently applies its own cooldown and probability. Its attempt timestamp is advanced **before** the probability roll—so a failed roll still consumes the configured interval.

It then selects one of **four** built-in WoW emotes: **ROAR, CHARGE, CHEER, FLEX**. The earlier version also included BORED, which was removed in a January 2026 code revision. If two instances watch the **same physical slot**, both may respond separately to the same button press. The addon forwards control to the original `UseAction`, but does not verify that the ability actually succeeded.

Configuration is stored in account-wide `ROEDDB`. The nested `slots` entries include each instance's `slot`, `chance`, `cd`, and `last` fields. The per-instance `last` timestamp also persists, which deserves special testing after restarting a client because `GetTime()` is session-relative.

## Relationship to other ROAR projects

[The earlier `roar` repository](https://github.com/babunidev/roar) and [`roarr`](https://github.com/babunidev/roarr) implement a simpler **single watched-slot** design. ROARRunlimited, created in December 2025, independently stores multiple numbered instances with separate odds and timing. A later project named RoarForever has its own history; shared naming should not silently merge their codebases or authorship.

## License, current limitations, and verification

The repository has an **MIT LICENSE**, credited to Babuni.dev. The source itself retains an older `babunigaming` author comment, which is part of historical source and was left untouched.

This README reflects static source inspection. The following are **not yet tested in the game**: filename-case loading, all slash commands, multi-instance output, restart persistence, behavior alongside other `UseAction` hooks, and cross-client emote token support. See the [roadmap](ROADMAP.md) for an actionable test checklist.

## Project documentation

- [Concept](CONCEPT.md) · [Interaction design](docs/DESIGN.md) · [Content](docs/CONTENT.md)
- [Architecture](docs/ARCHITECTURE.md) · [Decisions and alternatives](docs/DECISIONS.md) · [Narrative development journal](docs/DEVLOG.md)
- [Verification roadmap](ROADMAP.md) · [Changelog](CHANGELOG.md)
- [Human–AI collaboration](docs/AI_COLLABORATION.md) · [AI development instructions](AGENTS.md)

Documentation distinguishes Babuni.dev's authorship, demonstrable Git history, AI editorial work, and in-game behavior still awaiting verification.
