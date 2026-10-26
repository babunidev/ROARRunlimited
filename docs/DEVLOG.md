# ROARRunlimited — creative and technical development journal

**Creator / creative director:** Babuni.dev.  
**Prepared:** 2026-10-26 from the actual GitHub README, Lua source, manifest and selected dated commit diffs. It is a **retrospective reconstruction**, not a transcript of the original creative discussion.

## A step past the single roar

ROARRunlimited grows from a simple pleasure in making a World of Warcraft character seem more expressive. Instead of treating one button as the source of an occasional battle cry, the project makes a collection of action-bar buttons eligible to produce different spontaneous responses. An aggressive move can lead to a roar, another to an exuberant cheer, another to a proud flex. The player's actual actions remain intact; the extra behavior is flavor.

The creator's README famously reduces the idea to a compact image: **“ROARRunlimited turns your action bar into a war drum.”** That voice is valuable to preserve. A strong project journal should not erase a whimsical creative premise under generic technical documentation.

The earliest code and the later one share an intentional minimalism: built-in WoW emote tokens, slash-command configuration, no permanent GUI, and no automatic combat decisions. That experience is directly supported by source and README. The original private conversation explaining each chosen constraint is not visible in the inspected repository; this journal does not pretend otherwise.

## 14 December 2025: a first multi-slot version

Git commits on **2025-12-14** created a manifest (`a02cbaf`, shortly renamed to `ROARRunlimited.toc` in `d2c7d3c`) and uploaded an initial `ROARRunlimited.lua` (`47ffc04`). The file identified itself as **v1.0** and introduced a five-token battle pool: ROAR, CHARGE, CHEER, BORED and FLEX.

At that point, the `WATCH_SLOTS` table used **physical action-slot numbers as keys**, with a per-slot cooldown and probability. This already differentiated the work from earlier single-slot projects. The first registered slash command was `/roed`, which would remain the interface even when the internal storage structure changed.

The first code was not yet the final interface. The same day's `ad7d6a2` diff replaced `string.match` with `string.find` in the command parser, a meaningful compatibility correction for the declared Lua 5.0 target. It also changed the chance/timer commands from values applied across watched entries to configuration-specific controls. A later `58b9367` commit repaired an incomplete logout-event ending in a previous intermediate source snapshot.

Those are observable technical iterations. They demonstrate active problem-solving, but do not tell us exactly which human or AI proposed each fix or which tests prompted it.

## Still 14 December: the meaning of an instance

A particularly interesting design refinement appears in `fcba0bf`, dated **2025-12-14**. Before it, the table key and physical watched slot were closely coupled. After it, the addon stored entries under a chosen **instance number**, each containing its own `slot`, `chance`, `cd`, and `last`.

That gives the player a conceptual handle—`slot1`, `chance1`, and `timer1` always refer to the same configured behavior, even when instance 1 is reassigned from physical slot 13 to slot 24. It is a small but meaningful separation of **identity** from **game-control location**.

The newer code scans each configured instance on every `UseAction` call, testing whether its physical slot matches the one just pressed. This architecture also permits two instances to watch the same button. The source does not deduplicate their responses; whether producing two battle cries together is desired remains a creator question.

## 9 January 2026: “FIXED” and a tighter emote pool

A **2026-01-09** commit (`ce86437`) renamed the header **“ROARRunlimited v1.1 FIXED.”** Its diff normalized fields on stored entries when first loaded, made configuration setup more robust, reset the per-instance cooldown time on slot assignment, and trimmed the battle pool from five tokens to four by removing **BORED**. The TOC was updated to version 1.1 shortly afterward in `5919dcc`.

One should not equate “FIXED” in a source header with a verified release. The current repository still contains a notable packaging mismatch: `ROARRunlimited.toc` requests lowercase `roarrunlimited.lua`, while Git holds `ROARRunlimited.lua`. Some deployment environments distinguish those names. The case discrepancy is visible in the earliest manifest and persists in the current tree.

The source also persists the `last` value, which is derived from `GetTime()`. Since this is session-relative rather than a stable real-world timestamp, a saved entry can hold an elapsed-time value that makes no sense after a client restart. Meanwhile, because saved settings are lazily loaded only when the player uses the action hook or slash command, logging out without doing either may overwrite settings with initial values. These risks belong to honest technical documentation even when no player bug report exists.

## September–October 2026: a durable portfolio record

A **2026-09-18** commit (`ad85459`) generalized specific private-server wording to the broader **Vanilla WoW 1.12** client target. Several **2026-10-26** commits updated Babuni.dev attribution and added basic documentation links and skeletons.

In this deeper documentation pass, an AI assistant inspected the actual source and selected historic diffs, separated ROARRunlimited from the earlier `roar` and `roarr` projects, identified the packaging and persistence questions, and expanded the full project documentation. That is **editorial research and a documentation commit**, not the creation of the original concept and not a runtime code fix. No Lua/TOC changes or in-game tests were made.

## What the development record protects

The distinctive creative direction belongs to Babuni.dev: turning routine button presses into a playful, dynamic war drum. The AI contribution here is to make the code, its history and its uncertainties legible for future maintenance and an eventual portfolio narrative. The records do not assign unrecorded dialogue or motivation to either party.

**Verified through Git/source reading:** file history, instance model, four-token pool, default settings, command forms, capitalization mismatch, and potential persistence pitfalls. **Unverified in-game:** whether current package loads on the intended client, probability/cooldown behavior, duplicate-slot output, cross-addon compatibility, and any actual reported failure.
