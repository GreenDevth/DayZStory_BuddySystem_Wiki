# Chapter 2: Core Configuration Reference (`BuddySettings.json`)

> **[⬅️ Chapter 1: Architecture & Installation Guide](./01-Architecture-and-Setup.md)** | **[➡️ Chapter 3: Satellite Terminal, Insurance & Radio Shop](./03-DLC-Satellite-and-Insurance.md)**

File location:
`$profile:\DayZStory\AiBuddy\BuddySettings.json`

> [!IMPORTANT]
> **Boolean Values in DayZ JSON Files:**
> DayZ's `JsonFileLoader` serializes `bool` variables as integers: **`1` = `true` (Enabled)** and **`0` = `false` (Disabled)**. Always use `1` or `0` in `.json` files.

---

## 1. General & Economy Settings

| Variable Name (Key) | Type | Default | Valid Values / Options | Description & Behavior |
| :--- | :---: | :---: | :--- | :--- |
| `m_SystemEnabled` | `bool` (`0`/`1`) | `1` | `0` = Disable entire BuddySystem<br>`1` = Enable BuddySystem | Master switch for the entire mod. If set to `0`, no companions can be hired, spawned, or recalled. |
| `m_EconomyMode` | `int` | `0` | `0` = **Hardline Reputation** (Expansion Hardline points)<br>`1` = **eWallet Coins** (`DayZStory_eWallet` balance)<br>`2` = **Expansion Bank** (Expansion Market ATM balance)<br>`3` = **Free** (No cost / no requirement) | **Primary currency mode** used for hiring companions (`m_Cost`), checking minimum qualification (`m_RequireMinRep`), radio call-back fees (`m_CallBackCost`), and as the default currency for Satellite/Gravedigger modules whenever their `EconomyMode` is set to `-1`. |
| `m_Cost` | `int` | `500` | `0` or higher | Fallback hiring fee if an entry in `m_BuddyRoster` does not specify its own `m_Cost`. |
| `m_RequireMinRep` | `int` | `1000` | `0` or higher | Fallback minimum balance/reputation required to unlock hiring. |
| `m_EnableKeybind` | `bool` (`0`/`1`) | `0` | `0` = Disabled (must talk to a Contractor NPC)<br>`1` = Allow opening the hiring menu via keybind anywhere | Recommended to keep at `0` on survival servers so players must visit a Contractor NPC in a safezone. |
| `m_EnableCallBackCost` | `bool` (`0`/`1`) | `1` | `0` = Free call-back<br>`1` = Charge `m_CallBackCost` | Whether to charge the player when teleporting a separated companion back (`Call AI Back`). |
| `m_CallBackCost` | `int` | `50` | `0` or higher | Fee charged for `Call AI Back` when `m_EnableCallBackCost: 1`. |
| `m_EnableRadio` | `bool` (`0`/`1`) | `1` | `0` = Disable radio integration<br>`1` = Enable radio integration | Master toggle allowing handheld radio interaction with `DayZStory_Buddy_Radio`. |
| `m_LoadoutClass` | `string` | `"HumanLoadout.json"` | Any `.json` loadout filename in `ExpansionMod/Loadouts/` | Fallback loadout file used if a buddy entry in `m_BuddyRoster` has an empty `m_LoadoutClass`. |

---

## 2. Companion Lifecycle & Death Handling Settings

These variables define how your server handles companion persistence when players disconnect, dismiss their companion, or die:

| Variable Name (Key) | Type | Default | Valid Values / Options | Description & Behavior |
| :--- | :---: | :---: | :--- | :--- |
| `m_DeleteAIOnPlayerDisconnect` | `bool` (`0`/`1`) | `1` | `0` = Companion remains standing in the world when owner logs out<br>`1` = **Recommended:** Save companion & gear to disk and despawn immediately on logout | Setting to `1` saves server performance and prevents offline griefing. When the player reconnects, they can recall their companion with all gear intact for free. |
| `m_DeleteAIOnPlayerDismiss` | `bool` (`0`/`1`) | `0` | `0` = Dismissed companion becomes a roaming AI (`eAIFactionRoamingBuddy`) before auto-cleaning<br>`1` = Delete companion and save file immediately upon dismissal | Triggered when the player clicks **`DISMISS COMPANION`** in the Companion Management menu. Permanently terminates the contract and deletes `<steamid>.json`. |
| `m_AutoCleanDismissedAI` | `bool` (`0`/`1`) | `1` | `0` = Never auto-delete dismissed roaming AI<br>`1` = Auto-delete dismissed AI after `m_AutoCleanDismissedTime` | Active when `m_DeleteAIOnPlayerDismiss: 0`. Prevents dismissed companions from accumulating on the server. |
| `m_AutoCleanDismissedTime` | `int` | `900` | Time in seconds (`900` = 15 minutes) | How long a dismissed companion roams the map before being automatically despawned. |
| `m_DeleteAIOnPlayerDeath` | `bool` (`0`/`1`) | `1` | `0` = Leave companion active when owner dies<br>`1` = **Recommended:** Process companion according to `m_OwnerDeathBuddyMode` | Must be set to `1` for `m_OwnerDeathBuddyMode` to function properly when the owner dies. |
| `m_OwnerDeathBuddyMode` | `int` | `0` | **`0` = Satellite / Cemetery Lock (Default)**<br>**`1` = Release as Roaming AI**<br>**`2` = Safe Despawn & Recallable** | **Determines what happens to an active companion when the Player (Owner) dies:**<br>• **Mode `0`**: The companion's inventory is saved, the companion is safely despawned, and the save file is locked (`OwnerDeadState = 1`). After respawning, the player can claim all companion gear back in a private **`eAI_SupplyCrate`** at a **Satellite Terminal** (using Digital Insurance) or by excavating a grave at the **Gravedigger Cemetery**, then hire a new companion.<br>• **Mode `1`**: The companion leaves the player's group, becomes an independent roaming AI at the death site, and the save file is deleted immediately.<br>• **Mode `2`**: The companion is saved and safely despawned **without locking** (`OwnerDeadState = 0`). After respawning, the player can simply use a paired radio or Satellite Terminal to **Recall** the exact same companion with all their gear intact! |

---

## 3. Satellite Recovery & Digital Insurance Settings

> [!NOTE]
> When **`DayZStory_Buddy_Satellite`** is installed, values configured in `$profile:DayZStory/AiBuddy/DLC/Satellite/Setting.json` automatically synchronize and override the corresponding fields below.

| Variable Name (Key) | Type | Default | Valid Values / Options | Description & Behavior |
| :--- | :---: | :---: | :--- | :--- |
| `m_EnableRecoverySystem` | `bool` (`0`/`1`) | `1` | `0` = Hide Digital Insurance & gear claiming from Satellite Terminals<br>`1` = Enable Digital Insurance & Satellite gear recovery | Set to `0` if you want players to recover companion gear **exclusively by digging graves at the Gravedigger Cemetery** (or if you use `m_OwnerDeathBuddyMode: 2`). When set to `0`, the `[BUDDY INSURANCE POLICY]` button is automatically hidden on Satellite Terminals. |
| `m_RecoveryReputationCost` | `int` | `1000` | `0` or higher (`0` = Free claim) | Fee charged when claiming the companion's gear crate (`eAI_SupplyCrate`) at a Satellite Terminal or Gravedigger Cemetery. |
| `m_RecoveryEconomyMode` | `int` | `-1` | `-1` = Inherit `m_EconomyMode`<br>`0` = Hardline Rep, `1` = eWallet, `2` = Bank, `3` = Free | Currency used to pay the `m_RecoveryReputationCost` claim fee. |
| `m_InsuranceCost` | `int` | `1000` | `1` or higher | Price to purchase 1 Digital Insurance Policy (`PURCHASE DIGITAL POLICY (+1)`). |
| `m_InsuranceEconomyMode` | `int` | `-1` | `-1` = Inherit `m_EconomyMode`<br>`0` = Hardline Rep, `1` = eWallet, `2` = Bank, `3` = Free | Currency used to purchase Digital Insurance policies (can be configured independently from Radio Shop and hiring currencies). |
| `m_MaxInsurancePolicies` | `int` | `3` | `0` = Unlimited<br>`1+` = Maximum policies a player can stack | Maximum number of policies a player can hold at once. Each gear recovery claim consumes 1 policy (`-1 Policy`). |
| `m_InsuranceMenuIconPath` | `string` | `"DayZExpansion/Core/GUI/icons/hud/shield_64x64.edds"` | Any valid `.edds` or `.paa` path | Icon displayed inside the `BUDDY INSURANCE POLICY` sub-window. |
| `m_InsuranceHUDIconPath` | `string` | `"DayZExpansion/Core/GUI/icons/hud/persona_64x64.edds"` | Any valid `.edds` or `.paa` path | Icon displayed on the bottom-right player HUD when holding `>= 1` insurance policy. |
| `m_SatelliteTerminalClasses` | `array<string>` | `["DZS_SatelliteTerminal:component12", "Land_Radio_PanelPAS"]` | Array of `"ClassName"` or `"ClassName:ComponentName"` | World objects (and optional specific component after `:`) that display the `Access Satellite` action. |
| `m_ContractorObjectClasses` | `array<string>` | `[]` | Array of `"ClassName"` or `"ClassName:ComponentName"` | World objects/props that allow opening the Buddy Contractor recruitment menu without spawning an AI NPC. |

---

## 4. Player-to-Buddy Medical Healing, UI Branding & FriendlyList

| Variable Name (Key) | Type | Default | Description & Behavior |
| :--- | :---: | :---: | :--- |
| `m_HealItemBandages` | `array<string>` | `["bandagedressing", "rag", "splint"]` | Lowercase classnames of items in the player's inventory that can be consumed when clicking **`BANDAGE WOUNDS`** in the `Companion Medical Status` menu. Stops all bleeding cuts, heals broken legs, and restores `m_HealHPBandage` HP. |
| `m_HealItemTonics` | `array<string>` | `["sodacan_cola", ..., "epinephrine", "morphine", "salinebagiv", "tetracyclineantibiotics"]` | Lowercase classnames of items in the player's inventory consumed when clicking **`GIVE TONIC / HEAL`**. Restores `m_HealHPTonic` HP and `m_HealBloodTonic` Blood. |
| `m_HealHPBandage` | `float` | `15.0` | Companion HP restored per bandage action (out of 100 max HP). |
| `m_HealHPTonic` | `float` | `35.0` | Companion HP restored per tonic/heal action (out of 100 max HP). |
| `m_HealBloodTonic` | `float` | `1000.0` | Companion Blood restored per tonic/heal action (out of 5000 max Blood). |
| `m_MenuLogoPath` | `string` | `"DayZStory_Buddy_GUI/Data/band.paa"` | **Header Emblem Logo** displayed next to the title bar across all menus (`BuddyMenu`, `ManageBuddyMenu`, `BuddyRecoveryTerminalMenu`, `BuddyStatusMenu`, `BuddyRenameMenu`). Replace with your server's custom `.paa` or `.edds` logo if desired. |
| `m_MenuTitleText` | `string` | `"SURVIVOR MERCENARY & ESCORT CONTRACTOR"` | Header title displayed in the main `BuddyMenu` recruitment window. |
| `m_DisableExpansionBookKeyB` | `bool` (`0`/`1`) | `1` | `1` = Prevents the `B` key from opening the Expansion Book so it does not conflict with buddy hotkeys. |
| `AdminSteamIDs` | `array<string>` | `["76561198242071384"]` | List of SteamID64s with admin privileges for BuddySystem commands. |
| `FriendlyList` | `array<string>` | `["eAIFactionPlayerSurvivor", "eAIFactionCivilian", ...]` | List of Expansion AI Faction classnames treated as friendly by Contractors, Gravediggers, and Roaming Buddies (see **[Chapter 6](./06-Factions-Coordinates-and-Editor-Guide.md)**). |

---

## 5. Individual Companion Profiles (`m_BuddyRoster` — All 20 Fields)

The `"m_BuddyRoster": [ ... ]` array defines all recruitable companions (34 unique characters included by default). Each entry uses the `BuddyEntryConfig` schema:

| Field Name (Key) | Type | Recommended Range | Description & In-Game Effect |
| :--- | :---: | :---: | :--- |
| `m_ID` | `string` | Unique ID, e.g. `"boris_sniper"` | **Unique Primary Key** for this contractor. Must not duplicate any other entry. Used for save persistence and server-wide Exclusive Hiring (only 1 player can hire a specific contractor at a time). |
| `m_ClassName` | `string` | e.g. `"eAI_SurvivorM_Boris"` | Expansion AI character classname (`eAI_SurvivorM_*` or `eAI_SurvivorF_*`). |
| `m_DisplayName` | `string` | e.g. `"Boris 'Hawkeye'"` | Name displayed on recruitment cards and assigned to the companion when hired. |
| `m_Role` | `string` | e.g. `"Sniper"`, `"Field Medic"`, `"Breacher"` | Role badge shown in UI menus. **Important:** If `m_Role` contains the word `"Medic"` (case-insensitive, e.g. `"Field Medic"`), `DayZStory_Buddy_Medic` grants faster treatment times and shorter CPR cooldowns! |
| `m_IconPath` | `string` | `"DayZStory_Buddy_GUI/Data/<Name>.paa"` | Portrait image path shown in Recruitment, Companion Management, and Satellite Terminal menus (34 portraits included in `DayZStory_Buddy_GUI/Data/`). |
| `m_Description` | `string` | Multi-line text | Bio and specialty description shown in the recruitment detail panel. |
| `m_Cost` | `int` | `0+` | **Hiring Fee** deducted when the player recruits this companion. |
| `m_RequireMinRep` | `int` | `0+` | **Minimum Balance/Reputation Required** in the player's account to unlock hiring this companion. |
| `m_LoadoutClass` | `string` | e.g. `"HumanLoadout.json"` | Expansion loadout `.json` applied to the companion when first hired. |
| `m_AccuracyMin` | `float` | `0.0` – `1.0` | Minimum firing accuracy (`0.0` = 0%, `1.0` = 100%). |
| `m_AccuracyMax` | `float` | `0.0` – `1.0` | Maximum firing accuracy (displayed on the recruitment screen as `ACCURACY: ~XX%`). |
| `m_ThreatDistanceLimit` | `float` | `50.0` – `600.0` (meters) | Maximum sight range at which the companion detects and engages threats. |
| `m_NoiseInvestigationDistanceLimit` | `float` | `50.0` – `400.0` (meters) | Maximum distance at which gunshots or zombie noises alert the companion. |
| `m_MaxFlankingDistance` | `float` | `0.0` – `200.0` (meters) | Maximum distance the companion will maneuver to flank enemies during combat. |
| `m_SniperProneDistanceThreshold` | `float` | `0.0` = Disabled<br>`> 0.0` = Distance in meters (e.g. `120.0`) | **Sniper Specialty:** When engaging targets farther than this distance, the companion automatically drops into a **Prone** stance to fire. |
| `m_DamageMultiplier` | `float` | `0.5` – `3.0` (`1.0` = Normal) | Outgoing weapon/melee damage multiplier dealt by this companion. |
| `m_DamageReceivedMultiplier` | `float` | `0.1` – `2.0` (`1.0` = Normal) | Incoming damage multiplier taken by this companion (**lower = tankier**, e.g. `0.75` = takes 25% less damage; `0.0` = invincible). |
| `m_ShoryukenChance` | `float` | `0.0` – `1.0` (e.g. `0.05` = 5%) | Probability of triggering a special Shoryuken uppercut punch in close-quarters melee combat. |
| `m_ShoryukenDamageMultiplier` | `float` | `1.0` – `10.0` (`3.0` = 3x damage) | Damage multiplier applied when a Shoryuken punch lands. |
| `m_LootingBehavior` | `int` | `0`, `1`, `2`, `3` | **Automatic Looting Behavior:**<br>• `0` = `NONE` (Never auto-loots)<br>• `1` = `WEAPONS` (Weapons & ammo only)<br>• `2` = `MEDICAL` (Bandages & medical items)<br>• `3` = `ALL` (Upgrades gear and loots nearby items) |
