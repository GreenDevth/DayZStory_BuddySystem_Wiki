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

## 4. Importing Scenes, Targets & NPCs from DayZ Editor (`"EditorJsonFiles": []`) Across All Modules

All three world-spawning modules (**Contractors**, **Gravedigger**, and **Satellite**) support `"EditorJsonFiles": []` in their respective `Setting.json` files:

- **When `"EditorJsonFiles": []` is empty:** The module loads default settings and only spawns entries configured with a non-zero `Position` in `Setting.json` (any entry with `"Position": [0.0, 0.0, 0.0]` is safely skipped).
- **When filenames are listed in `"EditorJsonFiles": ["my_scene.json"]`:** The spawner parses the exported **DayZ Editor `.json`** file and automatically distinguishes between **decorative props**, **interactive targets**, and **NPCs** based on each object's classname (`"name"`):

### How DayZ Editor Objects Are Resolved Per Module (via Core `BuddyEditorSceneLoader`)

| Module & Scene Folder | Decorative Props | Interactive Targets & Markers in DayZ Editor | NPCs in DayZ Editor (`customString` Support) |
| :--- | :--- | :--- | :--- |
| **1. Contractors**<br>`$profile:\DayZStory\AiBuddy\DLC\Contractors\Scenes\` | Tables, chairs, tents, and `.p3d` static objects are spawned as scene props. | *(The Contractor NPC itself is the interaction target)* | • **Contractor NPC:** Place any character class inheriting from `eAIBase` (e.g., `eAI_SurvivorM_Boris`). Automatically spawned as an immortal stationary **Contractor NPC** in `eAIFactionContractors`, inheriting defaults from `Contractors[0]` (or overridden per-NPC via `customString` `"Name\|Loadout\|EmoteID"`). |
| **2. Gravedigger**<br>`$profile:\DayZStory\AiBuddy\DLC\Gravedigger\Graves\` | Cemetery gates, fences, lamps, and `.p3d` static objects are spawned as scene props. | • **Excavation Graves:** Place as many **`DZS_BuddyGrave`** objects as desired within `CemeteryRadius`. Players can excavate any of them using a borrowed shovel (`ActionDigBuddyGrave`). | • **Gravedigger NPC:** Place any character class inheriting from `eAIBase` (e.g., `eAI_SurvivorM_Rolf`). Automatically spawned as an immortal stationary **Gravedigger NPC** in `eAIFactionGravediggers`, inheriting defaults from `Gravediggers[0]` (or overridden per-NPC via `customString` `"Name\|Loadout\|EmoteID"`). |
| **3. Satellite**<br>`$profile:\DayZStory\AiBuddy\DLC\Satellite\Terminals\` | Communication tents, antennas, and `.p3d` static objects are spawned as scene props. | • **Satellite Terminal:** Place **`DZS_SatelliteTerminal`** or **`Land_Radio_PanelPAS`** to register an interactive Satellite Terminal.<br>• **Claim Box Marker:** Place an **`eAI_SupplyCrate`** within 50m of a terminal in DayZ Editor. The system **does not spawn an empty crate**; instead, it records that exact position and orientation as the nearest terminal's `ClaimBoxPosition`! | *(No NPCs)* |

### Per-NPC Customization in DayZ Editor via `customString` (`"Name|Loadout|EmoteID"`)

When placing an `eAI_*` character in **DayZ Editor**, you can customize that individual NPC's name, loadout, and idle emote directly inside its **`customString`** property using the pipe-separated format **`"Name|Loadout|EmoteID"`**:
- **Empty `""`:** Inherits `Name`, `Loadout`, and `EmoteID` from the first template entry (`Contractors[0]` or `Gravediggers[0]`) in `Setting.json`.
- **Name only (e.g. `"Boris Contractor"`):** Sets the NPC's name to `"Boris Contractor"` while inheriting `Loadout` and `EmoteID` from `Setting.json`.
- **Full override (e.g. `"Boris Contractor|MilitaryLoadout|44"`):** Sets the NPC's name to `"Boris Contractor"`, applies `"MilitaryLoadout"`, and plays `EmoteID = 44` (`ID_EMOTE_SALUTE`) on that specific NPC!

> [!TIP]
> **Using DayZ Editor Exclusively Without Duplicate Spawns:**
> If you place your NPCs and Targets inside a DayZ Editor `.json` file, simply set `"Position": [0.0, 0.0, 0.0]` on the first entry in `Contractors[]`, `Gravediggers[]`, or `Terminals[]` in `Setting.json`. The `[0,0,0]` entry won't spawn at the corner of the map, while its `"Loadout"`, `"EmoteID"`, and `"Name"` will still serve as the template for NPCs spawned from your DayZ Editor JSON!
