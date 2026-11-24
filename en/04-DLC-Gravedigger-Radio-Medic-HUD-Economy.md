# Chapter 4: Optional DLC Configuration Reference (`Contractors`, `Gravedigger`, `Radio`, `Medic`, `HUD`, `Economy`, `Controller`, `Admin`)

> **[⬅️ Chapter 3: Satellite Terminal, Insurance & Radio Shop](./03-DLC-Satellite-and-Insurance.md)** | **[➡️ Chapter 5: Death, Insurance & Recovery Matrix](./05-Death-Insurance-and-Recovery-Matrix.md)**

---

## 1. Contractor NPC Spawner (`DLC/Contractors/Setting.json`)

Managed by `DayZStory_Buddy_Core`. Spawns immortal Contractor NPCs (`eAIFactionContractors`) where players can browse and hire companions:

| Variable Name (Key) | Type | Default | Valid Values & Description |
| :--- | :---: | :---: | :--- |
| `Enable` | `bool` (`0`/`1`) | `1` | `1` = Spawn Contractor NPCs listed in `Contractors[]`, `0` = Disable spawning. |
| `IsPassive` | `bool` (`0`/`1`) | `0` | • `0` = Contractors defend themselves against zombies/hostiles.<br>• `1` = **100% Passive Mode:** Contractors never draw weapons or shoot anyone under any circumstances (ideal for Godmode safezones). |
| `SpawnRadius` | `float` | `5.0` | Clearance radius around spawn point (meters). |
| `Contractors[].Name` | `string` | `"Buddy Contractor"` | NPC group/display name. |
| `Contractors[].Position` | `[x, y, z]` | `[5120.0, 0.0, 5120.0]` | **Exact 3D Coordinates.** Uses your exact `Y` height without snapping to terrain so NPCs can stand on 2nd-floor balconies or custom platforms. Skipped if `[0.0, 0.0, 0.0]`. |
| `Contractors[].Orientation` | `[yaw, pitch, roll]` | `[0.0, 0.0, 0.0]` | Facing direction in degrees. |
| `Contractors[].Loadout` | `string` | `"HumanLoadout"` | Loadout filename from `ExpansionMod/Loadouts/`. |
| `Contractors[].ClassName` | `string` | `"eAI_SurvivorM_Boris"` | Expansion AI survivor classname. |
| `Contractors[].EmoteID` | `int` | `0` | Idle emote ID played 15 seconds after spawning (`0` = standing idle). |

---

## 2. Cemetery Gravedigger & Grave Excavation (`DLC/Gravedigger/Setting.json`)

Managed by `DayZStory_Buddy_Gravedigger`. Provides an immersive survival alternative (or complement) to Satellite Terminals for recovering fallen companion gear:

| Variable Name (Key) | Type | Default | Valid Values & Description |
| :--- | :---: | :---: | :--- |
| `Enable` | `bool` (`0`/`1`) | `1` | `1` = Enable Gravedigger NPCs and grave excavation, `0` = Disable DLC. |
| `IsPassive` | `bool` (`0`/`1`) | `0` | `1` = Gravedigger NPC never shoots anyone (100% Passive), `0` = Defends against threats. |
| `CemeteryRadius` | `float` | `60.0` | **Cemetery Zone Radius (meters)** around the Gravedigger NPC. If a player carries a borrowed shovel (`DZS_GravediggerShovel`) outside this radius, the shovel is automatically confiscated! |
| `DiggingTime` | `float` | `6.0` | Duration of the grave digging action in seconds. |
| `RecoveryReputationCost` | `int` | `1000` | Fee charged when excavating a companion's gear crate (`eAI_SupplyCrate`) from a grave (`0` = Free). |
| `RecoveryEconomyMode` | `int` | `-1` | Currency for the grave excavation fee (`-1` = Inherit `BuddySettings.json`, `0` = Hardline Rep, `1` = eWallet, `2` = Bank, `3` = Free). |
| `ShovelBorrowCost` | `int` | `250` | Fee charged when borrowing a `DZS_GravediggerShovel` from the Gravedigger NPC (`0` = Free). |
| `ShovelEconomyMode` | `int` | `-1` | Currency for the shovel rental fee (`-1` = Inherit `BuddySettings.json`, `0` = Hardline Rep, `1` = eWallet, `2` = Bank, `3` = Free). |
| `EditorJsonFiles` | `array<string>` | `[]` | List of **DayZ Editor `.json` filenames** placed in `$profile:DayZStory/AiBuddy/DLC/Gravedigger/Graves/` (e.g. `["cherno_cemetery.json"]`). Automatically spawns both decorative `.p3d` props and `DZS_BuddyGrave` objects! |
| `Graves[]` | `array<object>` | `[{Position, Orientation, ClassName}]` | Manually configured `DZS_BuddyGrave` coordinates (skipped if `Position` is `[0,0,0]`). |
| `Gravediggers[]` | `array<object>` | `[{Name, Position, Orientation, Loadout, ClassName, EmoteID}]` | Gravedigger NPC spawn entries (skipped if `Position` is `[0,0,0]`). |

---

## 3. Handheld Radio Communication (`DLC/Radio/Setting.json`)

Managed by `DayZStory_Buddy_Radio`:

| Variable Name (Key) | Type | Default | Valid Values & Description |
| :--- | :---: | :---: | :--- |
| `Enable` | `bool` (`0`/`1`) | `1` | `1` = Enable buddy actions on handheld radios, `0` = Disable. |
| `EnableCallConstructorsMenu` | `bool` (`0`/`1`) | `0` | `0` = Players must visit a Contractor NPC to hire a buddy.<br>`1` = Allow opening the hiring menu directly from a handheld radio anywhere. |
| `EnableDirectRadioRecall` | `bool` (`0`/`1`) | `1` | `1` = Allow recalling a saved companion directly via a handheld radio in the field (once paired).<br>`0` = Disable field radio recall (forces players to recall their companion exclusively at a **Satellite Terminal**). |
| `EnableCallBackCost` | `bool` (`0`/`1`) | `1` | `1` = Charge `CallBackCost` when using `Call AI Back` on a radio to teleport a separated companion. |
| `CallBackCost` | `int` | `50` | Fee charged for `Call AI Back`. |
| `RequireRadioPairing` | `bool` (`0`/`1`) | `1` | `1` = **Mandatory Satellite Pairing:** Players must bring a powered-ON radio to a Satellite Terminal and complete **`PAIR RADIO`** once per character life before using radio commands.<br>`0` = No pairing required; any powered-ON radio works immediately. |
| `AllowedRadioClasses` | `array<string>` | `[]` | Whitelist of allowed radio classnames. Leave empty `[]` to allow all `TransmitterBase` radios (`PersonalRadio`, `BaseRadio`, modded radios). |

---

## 4. Automated Field Medic Companion (`DLC/Medic/Setting.json`)

Managed by `DayZStory_Buddy_Medic`. Automatically checks owner medical states in priority order (**Unconscious CPR > Bleeding Bandage > Broken Leg Splint**):

| Variable Name (Key) | Type | Default | Valid Values & Description |
| :--- | :---: | :---: | :--- |
| `Enable` | `bool` (`0`/`1`) | `1` | `1` = Enable automated companion medical assistance, `0` = Disable. |
| `OnlyMedicRole` | `bool` (`0`/`1`) | `0` | `0` = **All companions can treat the owner**, while companions with `"Medic"` in `m_Role` treat faster with shorter cooldowns.<br>`1` = **Only companions with `"Medic"` in `m_Role`** perform medical rescues. |
| `RequireItems` | `bool` (`0`/`1`) | `1` | `1` = Bandaging consumes a `BandageDressing`/`Rag` and splinting consumes a `Splint` from the **companion's inventory**. **CPR is always free** (if the companion carries an `Epinephrine` auto-injector, they consume it to perform an instant high-speed injection reviving the owner to 100 Shock!).<br>`0` = Companions bandage and splint without needing items in their inventory. |
| `CheckIntervalSeconds` | `float` | `2.0` | How often the server checks player medical states (seconds, minimum `0.5`). |
| `MaxAssistDistance` | `float` | `40.0` | Maximum distance (meters) within which the companion will rush to assist. |
| `ThreatThreshold` | `float` | `0.4` | Maximum AI threat level (`0.0` – `1.0`) tolerated before treating (automatically increases to `0.75` if the owner is both **unconscious and bleeding**!). |
| `CPRCooldownSeconds` | `float` | `180.0` | CPR cooldown for standard companions (`180` = 3 minutes). |
| `MedicCPRCooldownSeconds` | `float` | `90.0` | CPR cooldown for **Medic** role companions (`90` = 1.5 minutes). |
| `ActionDurationSeconds` | `float` | `5.0` | Treatment animation duration for standard companions (seconds). |
| `MedicActionDurationSeconds` | `float` | `3.0` | Treatment animation duration for **Medic** role companions (seconds — reduced by another 40% when using Epinephrine). |
| `EnableCPR` | `bool` (`0`/`1`) | `1` | `1` = Enable unconscious CPR / Epinephrine revival. |
| `EnableBandage` | `bool` (`0`/`1`) | `1` | `1` = Enable bleeding wound bandaging. |
| `EnableSplint` | `bool` (`0`/`1`) | `1` | `1` = Enable broken leg splinting. |

---

## 5. 3D Overhead Marker & Insurance HUD (`DLC/HUD/Setting.json`)

Managed by `DayZStory_Buddy_HUD`:

| Variable Name (Key) | Type | Default | Valid Values & Description |
| :--- | :---: | :---: | :--- |
| `Enable` | `bool` (`0`/`1`) | `1` | `1` = Enable overhead 3D companion markers, `0` = Disable. |
| `HUDType` | `int` | `2` | `1` = Native Expansion Party 3D marker.<br>`2` = **Recommended:** Compact 16px BuddySystem 3D marker with physics Line-of-Sight (LOS) raycasting. |
| `MaxDistance` | `float` | `150.0` | Maximum render distance in meters (auto-hides within `1.5m` so it doesn't block close-up view). |
| `ShowIcon` | `bool` (`0`/`1`) | `1` | `1` = Show marker icon above companion's head. |
| `MarkerIconPath` | `string` | `""` | Custom `.edds` icon path (leave `""` for default). |
| `ShowName` | `bool` (`0`/`1`) | `1` | `1` = Display companion callsign on 3D marker. |
| `ShowDistance` | `bool` (`0`/`1`) | `1` | `1` = Display distance in meters. |
| `LOSCheck` | `bool` (`0`/`1`) | `1` | `1` = **Physics Line-of-Sight Raycast:** Hides the 3D marker when the companion is behind walls, buildings, rocks, or terrain hills. |
| `PartyMarkerLOSCheck` | `bool` (`0`/`1`) | `1` | `1` = Applies physics LOS raycasting to human party member 3D markers as well. |
| `ShowBuddyInPartyHUD` | `bool` (`0`/`1`) | `1` | `1` = Show companion health bar in the top-left Expansion Party HUD list. |
| `InsuranceIconPath` | `string` | `"DayZExpansion/Core/GUI/icons/hud/persona_64x64.edds"` | Icon path for the bottom-right Digital Insurance HUD indicator. |
| `InsuranceHUD_Position_X` | `float` | `0.0` | Horizontal offset from bottom-right reference (pixels). |
| `InsuranceHUD_Position_Y` | `float` | `65.0` | Vertical offset from bottom-right reference (pixels). |

---

## 6. Economy Rewards & Penalties (`DLC/Economy/Setting.json`), Controller (`DLC/Controller/Setting.json`) & Admin (`cfggameplay.json`)

### `DLC/Economy/Setting.json`
| Variable Name (Key) | Type | Default | Description |
| :--- | :---: | :---: | :--- |
| `Enable` | `bool` (`0`/`1`) | `1` | Enable companion kill rewards and friendly-fire penalties. |
| `RewardKillZombie` | `int` | `2` | Points awarded when your companion kills a zombie. |
| `RewardKillAnimal` | `int` | `5` | Points awarded when your companion hunts an animal. |
| `RewardKillPlayerAI` | `int` | `50` | Points awarded when your companion kills an enemy AI or hostile player. |
| `PenaltySystemEnabled` | `bool` (`0`/`1`) | `1` | Enable point deduction for killing companions. |
| `PenaltyKillOwnBuddy` | `int` | `500` | Penalty deducted when a player kills their own companion (betrayal). |
| `PenaltyKillStrangerBuddy` | `int` | `300` | Penalty deducted when a player kills another player's companion. |

### `DLC/Controller/Setting.json` & `DayZStory_Buddy_Admin`
- **`RadialMenuType`** (`DLC/Controller/Setting.json`): `0` = Auto-detect / Expansion `eAICommandMenu`, `1` = `DayZStory_RadialMenu`.
- **`BuddyAdmins`** (`cfggameplay.json` under `"PlayerData"` when `DayZStory_Buddy_Admin.pbo` is loaded):
  ```json
  "PlayerData": {
      "BuddyAdmins": ["76561198242071384"]
  }
  ```
