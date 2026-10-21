# Val Variant Updates

This file lists the changes the Val variant makes to the base DnD2024 mod. Add new changes at the top.

Path shorthand used below:
- `Public\…` = `Public\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\…`
- `Editor\…` = `Editor\Mods\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\…` (BG3 Toolkit source; kept in sync with `Public`)
- `Story\…` = `Mods\DnD2024_897914ef-5c96-053c-44af-0be823f895fe\Story\RawFiles\Goals\…`

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
