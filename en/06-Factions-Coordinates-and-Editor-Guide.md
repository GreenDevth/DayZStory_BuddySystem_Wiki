# Chapter 6: Faction Sync, FriendlyList, Coordinates & DayZ Editor Guide

> **[⬅️ Chapter 5: Death, Insurance & Recovery Matrix](./05-Death-Insurance-and-Recovery-Matrix.md)** | **[➡️ Chapter 7: Server Presets & Troubleshooting Guide](./07-Presets-and-Troubleshooting.md)**

---

## 1. Automatic Owner-to-Buddy Faction Synchronization

In **DayZ Expansion AI**, an `eAIGroup`'s **Faction** determines which players and AI patrols are treated as friendly or hostile.

### How `DayZStory_Buddy_Core` Resolves and Persists Companion Factions:
Whenever a companion is hired, recruited, or recalled, `BuddyPlugins.c` assigns their faction using a 4-tier priority resolution:

1. **Priority 1 — `DayZStory_PlayerFaction` Integration:**
   - Reads `$profile:DayZStory/PlayerFaction/PlayerData/<SteamID>.json` for the hiring player.
   - Maps the player's role/faction name to the corresponding `eAIFaction` class:
     - `"Military"` -> `"eAIFactionRoleMilitary"`
     - `"Medic"` -> `"eAIFactionRoleMedic"`
     - `"Police"` -> `"eAIFactionRolePolice"`
     - `"Bandit"` / `"Raider"` -> `"eAIFactionRaiders"`
     - `"Mercenary"` -> `"eAIFactionMercenaries"`
     - `"Survivor"` / `"Civilian"` -> `"eAIFactionCivilian"` (or any exact `eAIFaction*` classname stored in the file).
2. **Priority 2 — Owner's Active `eAIGroup` Faction:**
   - If the player's current group has a specific faction assigned, the companion inherits that exact faction class.
3. **Priority 3 — Persisted `"Faction"` Field in `<SteamID>.json`:**
   - Every companion save writes the resolved faction classname into `$profile:DayZStory/AiBuddy/BuddyData/<SteamID>.json`:
     ```json
     {
         "ClassName": "eAI_SurvivorF_Linda",
         "BuddyName": "Sophia",
         "ProfileID": "linda_allrounder",
         "Faction": "eAIFactionCivilian",
         "OwnerDeadState": 0
     }
     ```
4. **Priority 4 — Default Safe Fallback:**
   - Defaults to `"eAIFactionCivilian"`, ensuring friendly relations with survivors and safezone NPCs.

---

## 2. Configuring `FriendlyList` in `BuddySettings.json`

The `"FriendlyList"` array in `BuddySettings.json` registers mutual friendly relations between your server's AI factions and:
1. **Contractor NPCs (`eAIFactionContractors`)**
2. **Gravedigger NPCs (`eAIFactionGravediggers`)**
3. **Roaming Dismissed Buddies (`eAIFactionRoamingBuddy`)**

If you use custom Expansion AI factions for safezone guards or quest NPCs, add their exact `eAIFaction*` classnames to `"FriendlyList"`.

> [!WARNING]
> **Safezone Passive Mode (`"IsPassive": 1`):**
> Even when a player's faction is in `FriendlyList`, Expansion AI's built-in self-defense will cause Contractor or Gravedigger NPCs to retaliate if a player shoots a companion or another player in their line of sight while `"IsPassive": 0`. For safezones, set **`"IsPassive": 1`** in `DLC/Contractors/Setting.json` and `DLC/Gravedigger/Setting.json` so NPCs remain 100% peaceful.

---

## 3. Coordinates (`Position` & `Orientation`) and Popular `EmoteID`s

- **Exact 3D Height for NPCs & Graves:** `Contractors`, `Gravediggers`, and `Graves` use your exact `[X, Y, Z]` coordinates without forcing terrain `SurfaceY`, allowing placement inside multi-story buildings or on elevated platforms.
- **Placeholder Rule (`[0.0, 0.0, 0.0]`):** Any entry with `"Position": [0.0, 0.0, 0.0]` is automatically skipped on server startup.

### Popular `EmoteID` Values for Contractors & Gravediggers
| `EmoteID` | DayZ Constant | Animation Description |
| :---: | :--- | :--- |
| **`0`** | *(Default)* | Standard standing posture |
| **`10`** | `ID_EMOTE_CAMPFIRE` | Sitting cross-legged on the ground |
| **`11`** | `ID_EMOTE_SITA` | Sitting relaxed on the ground (Style A) |
| **`12`** | `ID_EMOTE_SITB` | Sitting with knees up (Style B) |
| **`1`** | `ID_EMOTE_GREETING` | Friendly wave |
| **`9`** | `ID_EMOTE_TAUNTELBOW` | Crossed arms / tough stance |
| **`44`** | `ID_EMOTE_SALUTE` | Military salute |
| **`58`** | `ID_EMOTE_LISTENING` | Hand to ear listening |

---

## 4. Importing Cemetery Scenes from DayZ Editor (`.json`)

To build a custom cemetery scene with multiple excavatable graves in **DayZ Editor**:
1. Place decorative props and as many **`DZS_BuddyGrave`** objects as you want within `CemeteryRadius` of your Gravedigger NPC.
2. Export the scene from DayZ Editor as a **`.json`** file (e.g., `cherno_cemetery.json`).
3. Copy `cherno_cemetery.json` into:
   `$profile:\DayZStory\AiBuddy\DLC\Gravedigger\Graves\cherno_cemetery.json`
4. Add the filename to `"EditorJsonFiles"` in `$profile:\DayZStory\AiBuddy\DLC\Gravedigger\Setting.json`:
   ```json
   "EditorJsonFiles": [
       "cherno_cemetery.json"
   ]
   ```
