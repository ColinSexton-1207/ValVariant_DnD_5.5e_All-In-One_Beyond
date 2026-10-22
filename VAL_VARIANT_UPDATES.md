# Val Variant Updates

This file lists the changes the Val variant makes to the base DnD2024 mod. Add new changes at the top.

Path shorthand used below:
- `Public\…` = `Public\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\…`
- `Editor\…` = `Editor\Mods\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\…` (BG3 Toolkit source; kept in sync with `Public`)
- `Story\…` = `Mods\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\Story\RawFiles\Goals\…`

---

## 2026-10-21: Merged the latest upstream changes

### Why
Brings in Yoonmoonsik's latest updates (up to `3a702257`, "Update Spanish localization for firearms content") while keeping all the Val variant changes.

### How the merge was done
This repo started from a snapshot of upstream with no shared git history, so git reported every overlapping file as a conflict. The snapshot was matched to upstream commit `1c7aad8c` (the only differences were the Val edits), and each of the 42 conflicted files was merged three-way using that commit as the base:
- **Upstream changes kept** where only upstream touched a file: races, weapons, localization, root templates.
- **Val changes kept** where only we touched a file: stats, feats, story, READMEs, logo, thumbnail.
- **Both kept** in `Spell_Target.txt` and `Target.stats`, which merged cleanly.
- **`meta.lsx`**: kept our name, author, description and `MD5`. Took upstream's higher `Version64` (`145944796899311616`) so the build isn't seen as older than the original.

Check: the merged result differs from upstream in exactly the same 19 files and lines as the Val edits differ from `1c7aad8c` (+102/−323).

### How to test
- Sync the repo to the Toolkit data folder, repack, and repeat the earlier tests: two slot spells per turn, vanilla short rests, icons on Jump, Shove, Topple, Dodge and Distract.

---

## 2026-10-21: Fixed missing icons (Jump, Shove, Topple, Dodge, Distract and other custom icons)

### Why
The mod's icon atlas (`Public\…\GUI\bg3dnd_hotbarIcons.lsx`, 410 icons) loads its texture from `Assets/Textures/Icons/bg3dnd_hotbarIcons.dds`. The game resolves that path inside `Public\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\`, but the texture was only in `Mods\…\Assets\Textures\Icons\`. Every action with a custom icon showed up blank. While the original mod.io pak was installed it supplied the texture, so the problem only appeared after the original was removed.

### Changes
| File | Change |
|---|---|
| `Public\Assets\Textures\Icons\bg3dnd_hotbarIcons.dds` | Added. A copy of `Mods\…\Assets\Textures\Icons\bg3dnd_hotbarIcons.dds` (identical MD5). |
| Toolkit data folder (`C:\Program Files (x86)\Steam\steamapps\common\Baldurs Gate 3\Data\`) | The Toolkit packs from here, not from this repo. Its folders are plain copies, not symlinks. Added the texture to its `Public`. Moved the stale duplicate files out of its `Mods` into `Desktop\bg3dnd-toolkit-mods-duplicate-backup`. All four folders (`Public`, `Mods`, `Editor\Mods`, `Projects`) now match the repo. |
| `Mods\…\meta.lsx` | Synced from the Toolkit copy (`Version64` → `…988`). |

### Deliberately left in place
- **The copy in `Mods\…\Assets\Textures\Icons\`.** The game doesn't read it from there.
- **`.gitignore`'s `**.DDS` rule.** The texture is 22 MB and isn't tracked by git, so a fresh clone won't have it. Copy it to the `Public` path before packing.
- **Repo and Toolkit are separate copies.** Edits made in the repo don't reach the pak until they're copied into the Toolkit's data folder, and Toolkit edits don't reach the repo either. Sync them before packing and before committing.

### How to test
- After repacking, Jump, Shove, Topple, Dodge and Distract have icons on the hotbar, and so do other actions with custom icons from this mod.

---

## 2026-10-21: Removed a stale duplicate of the mod's data from `Mods\` and updated the description and thumbnail

### Why
Commit `d3ae992` added 1,141 files under `Mods\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\`. They were an extracted copy of the original mod.io pak (`RootTemplates`, `Stats`, `Feats`, `Progressions`, and so on), which belongs in `Public\`. The copy:
- was an **older** upstream version than `Public\`. It was missing the Opportunity Attack and Commander's Strike D8 entries that upstream GitHub and `Public\` have.
- had none of the Val variant edits, and its `SpellSet.txt` still listed the deleted `Shout_HitPointDice`. It hadn't been packed yet, but it would have been included in the next build.

### Changes
| File | Change |
|---|---|
| `Mods\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\…` | Removed the 1,141 files added in `d3ae992`. Recoverable with `git checkout d3ae992 -- <path>`. |
| `Mods\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\meta.lsx` | `Description` (set in the Toolkit): now "Custom playthrough edit of bg3dnd by Yoonmoonsik. All original work and rights belong to the original author…". |
| `Mods\…\mod_publish_logo.png`, `Projects\…\thumbnail.png` | New logo and thumbnail (set in the Toolkit). |

### Deliberately left in place
- Everything that was in `Mods\` before `d3ae992`: `meta.lsx`, `mod_publish_logo.png`, `Story`, `Localization`, `Scripts`, `Levels`, `GUI\metadata.lsf`, and the `.DDS` icons (ignored by `.gitignore`).

### How to test
- After saving settings in the Toolkit, `Mods\DnD2024_…\` should not contain `Stats` or `RootTemplates`. If it does, the duplicate is back.

---

## 2026-10-21: Fixed infinite loading at the mod verification screen

### Why
After the original *DnD 5.5e All-in-One BEYOND* pak was removed, the game hung forever at the mod verification screen. `meta.lsx` still had the original mod.io pak's checksum (`MD5`), and a rebuilt pak can never match it. While the original pak was still installed, its matching checksum let verification pass, but the game was loading the original instead of this build.

### Changes
| File | Change |
|---|---|
| `Mods\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\meta.lsx` | `MD5`: `632fa963…` (the original mod.io pak) → empty. On the next pack the packer filled it in again with this build's checksum (`cac5ac4a…`) and raised `Version64` to `…987`. Expect both to change on every pack. |
| `README.md` | Added Installation steps (unsubscribe from the original both in-game and on mod.io) and a Double Install troubleshooting section. |

### Deliberately left in place
- **`FileSize` (`97381861`)** is still the original pak's size. Verification passes with it as-is, so it was left alone. If the verification hang comes back after a pack, clear this next.

### How to test
- With only this build's pak in the Mods folder, the game gets past mod verification and lists the mod as "Val's Variant: DnD 5.5e All-In-One Beyond".

---

## 2026-10-21: Renamed the mod and unlinked it from mod.io

### Why
The Val variant is a separate fork, so it shouldn't show up under the original name or publish over the original mod.io page.

### Changes
| File | Change |
|---|---|
| `Mods\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\meta.lsx` | `Name`: `DnD 5.5e All-in-One BEYOND` → `Val's Variant: DnD 5.5e All-In-One Beyond`. |
| same | `Author`: `Yoonmoonsik` → `Valtorei (Original: Yoonmoonsik)`. |
| same | `PublishHandle`: `4419649` → `0`, so the mod is no longer tied to the original mod.io listing. |

### Deliberately left in place
- **The mod `UUID` (`897914ef-…`) and folder names.** Changing them would break existing saves that reference the base mod.
- **The description and all other original author credits.**

### How to test
- In the mod manager, the mod should be listed as "Val's Variant: DnD 5.5e All-In-One Beyond" by "Valtorei (Original: Yoonmoonsik)".
- Existing saves made with the base mod should still load without a missing-mod warning.

---

## 2026-10-21: Removed the short rest Hit Point Dice mechanic

### Why
The base mod replaced vanilla short rest healing with a "Use Hit Point Dice" action: a short rest healed only 1 HP, and you spent Hit Dice to heal the rest. This also broke the "short rest heals to full" custom game setting. The Val variant goes back to vanilla short rest behavior.

### How the mechanic worked
1. **Override**: the mod overrode vanilla's `ShortResting` passive. On short rest it ran `RegainHitPoints(1);ApplyStatus(SHORT_RESTING,100,20)` in place of vanilla healing.
2. **Unlock**: the `SHORT_RESTING` status unlocked the `Shout_HitPointDice` action out of combat. It was removed on combat or turn start.
3. **Spend**: `Shout_HitPointDice` (with 06/08/10/12 variants) spent a `HitPointDice` resource to regain 1dX + Constitution modifier.

### Changes
| File | Change |
|---|---|
| `Public\Stats\Generated\Data\Passive.txt` (and `Editor\Stats\Stats\Passive.stats`) | `ShortResting`: removed the overridden `StatsFunctors`, `DisplayName`, `Description`, `Icon`, `PriorityOrder`, `Properties` and the `UnlockSpellVariant(Shout_HitPointDice…)` boosts, so vanilla's healing and text apply again. Only the per-level `HitPointDice08` grants for non-mod classes remain. |
| `Public\Stats\Generated\Data\Spell_Shout.txt` (and `Editor\Stats\SpellData\Shout.stats`) | Deleted `Shout_HitPointDice` and its `06`/`08`/`10`/`12` variants. |
| same | `Shout_SpeedyRecovery` (Durable feat) inherited from `Shout_HitPointDice`. Copied the fields it relied on (healing, targeting, animation, intent, flags) into it directly and dropped the `using`. It works the same as before. |
| `Public\Stats\Generated\Data\Status_BOOST.txt` (and `Editor\Stats\StatusData\Status_BOOST.stats`) | Deleted the `SHORT_RESTING` status. |
| `Public\Stats\Generated\SpellSet.txt` (and `Editor\Stats\SpellData\SpellSet.stats`) | Removed `Shout_HitPointDice` from `CommonPlayerActions` and `CommonSummonActions`. |
| `README_Modio.html`, `README_NexusMods.txt` | Removed "Hit Point Dice" from the Rest and Recovery feature list. |
| `README.md` | Added a Save Compatibility section. |

### Deliberately left in place
- **The `HitPointDice06`–`12` action resources and the class progression grants.** Sangromancy, Durable (Speedy Recovery), Dwarven Fortitude, Lifedrinker and the ranger companions still spend them. Hit Dice still show on the resource panel.
- **The `HIT_POINT_DIE_USE_*` statuses** used by Lifedrinker, and the Hit Dice icons.
- **The `ShortResting` passive entry itself.** It still grants Hit Dice to non-mod classes and is vanilla's passive, so every character already has it.

### How to test
- Take damage, then short rest. Healing should match vanilla, including the "short rest heals to full" custom setting.
- There should be no "Use Hit Point Dice" action on the hotbar or in common actions.
- Take the Durable feat and use Speedy Recovery in combat. It should spend a Hit Die and heal 1dX + CON.
- Load an existing save. If the old action was on the hotbar, that slot may be empty and can be removed.

---

## 2026-10-21: Removed "One Spell With a Spell Slot Per Turn"

### Why
The base mod enforces the 2024 rule that a creature can cast only one spell that uses a spell slot per turn. The Val variant drops this rule. You can now cast more than one slot spell per turn, for example with the action and the bonus action.

### How the rule worked
1. **Grant**: a story goal gave every player the passive `OneSpellWithASpellSlotPerTurn`.
2. **Trigger**: when you cast a spell using `SpellSlot`, `WarlockSpellSlot` or `ShadowSpellSlot`, the passive applied the status `ONE_SPELL_WITH_A_SPELL_SLOT_PER_TURN` for 1 turn.
3. **Block**: that status added a cost of 1 `OneSpellWithASpellSlotPerTurn` to every slot spell. No character ever has that action resource, so all other slot spells became uncastable until the next turn.
4. **Other sources**: Divine Smite (every reaction variant) and Invoke Duplicity's slot spells applied the same status directly.
5. **Exemptions**: about 190 `ModifyUseCosts(Replace,OneSpellWithASpellSlotPerTurn,0,-1,…)` boosts let specific free casts ignore the extra cost. Examples include Hunter's Mark through Favored Enemy and Mind Magic.

### Changes
| File | Change |
|---|---|
| `Story\bg3dnd_ApplyPassive.txt` | Deleted the block that gives players `OneSpellWithASpellSlotPerTurn`. |
| `Public\Stats\Generated\Data\Status_BOOST.txt` (and `Editor\Stats\StatusData\Status_BOOST.stats`) | `ONE_SPELL_WITH_A_SPELL_SLOT_PER_TURN`: removed its `Boosts`. This is what actually lifts the restriction. |
| `Public\Stats\Generated\Data\Passive.txt` (and `Editor\Stats\Stats\Passive.stats`) | `OneSpellWithASpellSlotPerTurn`: removed `StatsFunctorContext`, `Conditions`, `StatsFunctors` and the `using "EquippingWeapons"` parent, and set `Properties` to `IsHidden`. The parent had to go too, because otherwise the passive would inherit EquippingWeapons' on-equip action-point refund. |
| `Public\Stats\Generated\Data\Interrupt.txt` (and `Editor\Stats\Stats\Interrupt.stats`) | Removed `ApplyStatus(OBSERVER_OBSERVER,ONE_SPELL_WITH_A_SPELL_SLOT_PER_TURN,100,1);` from all 30 Divine Smite variants. |
| `Public\Stats\Generated\Data\Spell_Target.txt` (and `Editor\Stats\SpellData\Target.stats`) | Removed `ApplyStatus(SELF,ONE_SPELL_WITH_A_SPELL_SLOT_PER_TURN,100,1);` from the 6 Invoke Duplicity slot spells. |
| `Public\Feats\Feats.lsx`, `Public\Feats\FeatDescriptions.lsx` (and `Editor\Feats\Feats.tbl`, `FeatDescriptions.tbl`) | Removed the **Swift Witchcraft** feat. Its only purpose was to bypass this rule. |

### Deliberately left in place (now inert)
- **The `OneSpellWithASpellSlotPerTurn` passive and status entries.** Characters in existing saves already have the passive, so the entries stay defined. Both now do nothing.
- **The `OneSpellWithASpellSlotPerTurn` ActionResourceDefinition** (`Public\ActionResourceDefinitions\ActionResourceDefinitions.lsx` and the `Editor` `.tbl`). The ~190 exemption boosts still name this resource. With the status gone they replace a cost that never exists, which is harmless.
- **The `ModifyUseCosts(Replace,OneSpellWithASpellSlotPerTurn,…)` exemptions** in `Passive.txt` and `Status_BOOST.txt`.
- **The `SpellScrollUsage` passive.** It only sets the duration of the now-unused status.
- **`'ONE_SPELL_WITH_A_SPELL_SLOT_PER_TURN'` inside the smites' `EnableCondition` `HasAnyStatus({…})`.** This branch is now always false.
- **The `SwiftWitchcraft` passive, `Shout_SwiftWitchcraft` spell and `SWIFT_WITCHCRAFT` status.** Only the feat was removed. These stay so saves where someone took the feat don't reference missing stats.

### How to test
- Start a new game. The character shouldn't have the "One Spell With a Spell Slot Per Turn" passive.
- In combat, cast a leveled spell with the action and another with the bonus action, such as Guiding Bolt and Healing Word. Both should cast.
- Divine Smite, then cast a leveled spell on the same turn. This should be allowed.
- Swift Witchcraft shouldn't appear in feat selection.
- Load an existing save. Slot spells shouldn't be blocked, even though the old passive may still be attached and hidden.
