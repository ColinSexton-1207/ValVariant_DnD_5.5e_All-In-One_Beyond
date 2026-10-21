# bg3dnd - Personal Custom Build

This repository is a **personal fork / custom edit** of the [`bg3dnd`](https://github.com/Yoonmoonsik/bg3dnd) mod for *Baldur's Gate 3*. 

> **Disclaimer:** All original code, assets, and design rights belong to the original author, **[Yoonmoonsik](https://github.com/Yoonmoonsik)**. This repository exists solely for my personal playthrough tweaks and version control. It is not intended to replace or infringe upon the original mod.

---

## 🔗 Original Mod & Credits

* **Original Mod Repository:** [Yoonmoonsik/bg3dnd](https://github.com/Yoonmoonsik/bg3dnd)
* **Original Author:** [Yoonmoonsik](https://github.com/Yoonmoonsik)
* **Nexus Mods Profile:** [yoonmoonsik43](https://www.nexusmods.com/profile/yoonmoonsik43)
* **Support the Original Creator:** [Ko-fi](https://ko-fi.com/yoonmoonsik) | [PayPal](https://paypal.me/yoonmoonsick)

---

## 🛠️ Personal Playthrough Modifications

Below is a summary of the custom changes made in this build compared to the original mod:

* **[Change 1]:** *Removed the one spell cast w/ a spell slot per turn rule so that Sorcerers and Wizards can pop off again like vanilla while maintaining the new features of DnD All-In-One Beyond.*
* **[Change 2]:** *Removed the limitation of hit point dice during short rests so that way the vanilla short rest healing works as intended again. This just makes things less tedious when you're playing alone or with friends that don't warn you they short rest.*
* **[Future Expansion]:** *Will probably continue to tweak as I progress teh story.*

---

## 💾 Save Compatibility

These changes appear to be compatible with existing *DnD Beyond All-In-One* saves. Nothing game-breaking is removed: the Hit Point Dice resources, class progressions, and the features that use them (Sangromancy, Durable, Dwarven Fortitude, Lifedrinker, etc.) are all still intact. Only the short rest "Use Hit Point Dice" action and its rest status were removed.

* If the old *Use Hit Point Dice* action was on your hotbar, that slot may show up empty or broken. Just remove it.
* As always, back up your save before swapping mod versions.

> **⚠️ Do not install this alongside the original *DnD 5.5e All-in-One Beyond*.** This build uses the same mod UUID, folder name, and internal content IDs as the original, so the two will overwrite each other and only one (or a broken mix of both) will actually load. The upside of the shared UUID is that this build is a drop-in replacement: existing saves load without a missing-mod warning, and you can swap back the same way.

---

## 📦 Installation

1. **Unsubscribe from the original *DnD 5.5e All-in-One BEYOND* in both places:**
   * **In-game:** open the Mod Manager, find *DnD 5.5e All-in-One BEYOND*, and unsubscribe. Just disabling it is not enough.
   * **On [mod.io](https://mod.io/g/baldursgate3):** log in with the same account and unsubscribe there too. If you stay subscribed on either side, mod.io will quietly re-download the original the next time the game starts.
2. Close the game completely.
3. Open `%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\Mods` and delete the original pak. The mod.io download is named something like `dnd2024_897914ef-5c96-053c-44a-xxxx.pak` (random letters at the end).
4. Copy this build's pak, `DnD2024_897914ef-5c96-053c-44af-0be823f895fe.pak`, into that same folder.
5. Launch the game. In the Mod Manager, the mod should be listed as **"Val's Variant: DnD 5.5e All-In-One Beyond"**.

---

## 🩺 Troubleshooting: Double Install

**Symptoms:**
* The game is stuck loading forever on the mod verification screen.
* The mod shows up as *DnD 5.5e All-in-One BEYOND* instead of *Val's Variant*.
* Changes from this build are missing in-game (you're limited to one spell slot spell per turn again, or short rests ask you to spend Hit Point Dice).

All of these mean the original pak is installed or got downloaded again. To fix it:

1. Close the game. If it's frozen, end `bg3_dx11.exe` (or `bg3.exe`) in Task Manager.
2. Open `%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\Mods`. There should be only **one** pak with `897914ef` in its name: `DnD2024_897914ef-5c96-053c-44af-0be823f895fe.pak`. Delete any other one. The original usually has random letters at the end of its name.
3. Make sure you're unsubscribed from the original **both in-game and on mod.io** (see Installation). If the duplicate keeps coming back, one of these is still subscribed.
4. Delete the contents of `%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\LevelCache`. If both versions ran at the same time, this cache can hold a mix of the two. The game rebuilds it, so the next launch will be slower.
5. Still stuck at mod verification? Back up `%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\PlayerProfiles\Public\modsettings.lsx` and open it in a text editor. Find the entry with `Val&apos;s Variant` in its name. Directly above that line is `<attribute id="MD5" ... value="..."/>`. Change the value to empty (`value=""`), save, and launch again.
6. If an old save still acts strangely, try a new game. A save made while both versions were loaded can have the mixed content baked in.

---

## 📜 Usage & License

If you are interested in the base **BG3 DnD 5.5e Mod**, please visit and support the official repository created by **Yoonmoonsik** linked above.

> Note that AI is occassionally used to help track changes between versions and documentation.