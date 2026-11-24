# Chapter 3: Satellite Terminal, Digital Insurance & Radio Shop (`DayZStory_Buddy_Satellite`)

> **[⬅️ Chapter 2: Core `BuddySettings.json` Reference](./02-Core-BuddySettings.md)** | **[➡️ Chapter 4: Optional DLC Configuration Reference](./04-DLC-Gravedigger-Radio-Medic-HUD-Economy.md)**

File location:
`$profile:\DayZStory\AiBuddy\DLC\Satellite\Setting.json`

---

## 1. 3-View Satellite Terminal UI Architecture

When a player interacts with a Satellite Terminal (`DZS_SatelliteTerminal` or `Land_Radio_PanelPAS`), the **Satellite Emergency Terminal** opens in a clean Vanilla DayZ translucent layout divided into 3 dedicated views:

1. **View 0 — Main Terminal Hub (`SATELLITE EMERGENCY TERMINAL`)**:
   - Displays Companion Portrait, Callsign, Role, and Field Status.
   - Displays **Handheld Transceiver** status (requires holding a **Powered-ON** radio in hands), **Satellite Frequency Link** status (`PAIRED` / `UNPAIRED`), and **Survivor Account Balance** (automatically displays dual currencies side-by-side if Radio Shop and Insurance use different economy modes).
   - Main action button for **`PAIR RADIO`** and **`RECALL BUDDY`** (turns Emerald Green **`READY — CALL BUDDY`** with a notification chime once satellite lock succeeds).
2. **View 2 — Radio Equipment Shop (`RADIO EQUIPMENT SHOP`)**:
   - Full sub-window with `< BACK` navigation, 1-column equipment list, **live 3D item preview**, bottom-right stock badge (`IN STOCK: 5` decreasing per purchase to red `OUT OF STOCK`), condition range (`70% - 100%`), price, and purchase button.
3. **View 1 — Buddy Insurance Policy (`BUDDY INSURANCE POLICY`)**:
   - Full sub-window displaying active policy count (`ACTIVE / 1 / 3 POLICIES`), recovery clearance fee, **`PURCHASE DIGITAL POLICY (+1)`** button, and **`CLAIM COMPANION GEAR CRATE`** button.

---

## 2. Root Configuration Dictionary (`BuddyRecoveryTerminalConfig`)

| Variable Name (Key) | Type | Default | Valid Values / Options | Description & Behavior |
| :--- | :---: | :---: | :--- | :--- |
| `Enable` | `bool` (`0`/`1`) | `1` | `0` = Disable DLC & do not spawn terminals<br>`1` = Enable DLC & spawn terminals | Master switch for the Satellite DLC. |
| `SpawnRadius` | `float` | `5.0` | `1.0+` (meters) | Clearance radius around terminal spawn positions. |
| `RecoveryReputationCost` | `int` | `1000` | `0+` (`0` = Free recovery) | **Clearance Fee** charged when claiming a companion's gear crate (`eAI_SupplyCrate`) at the terminal. |
| `RecoveryEconomyMode` | `int` | `-1` | `-1` = Inherit `m_EconomyMode` from `BuddySettings.json`<br>`0` = Hardline Reputation<br>`1` = eWallet Coins<br>`2` = Expansion Bank<br>`3` = Free | **Currency used for the gear recovery fee** (`RecoveryReputationCost`). |
| `InsuranceCost` | `int` | `1000` | `1+` | **Price per Digital Insurance Policy (`+1 Policy`)**. |
| `InsuranceEconomyMode` | `int` | `-1` | `-1` = Inherit `m_EconomyMode` from `BuddySettings.json`<br>`0` = Hardline Reputation<br>`1` = eWallet Coins<br>`2` = Expansion Bank<br>`3` = Free | **Currency used to purchase Digital Insurance policies.** Can be set independently from the Radio Shop currency (e.g., `1` for eWallet Coins while Radio Shop uses `0` for Hardline Reputation). |
| `MaxInsurancePolicies` | `int` | `3` | `0` = Unlimited<br>`1+` = Max stackable policies | Maximum number of Digital Insurance policies a player can hold at one time. |
| `InsuranceMenuIconPath` | `string` | `"DayZExpansion/Core/GUI/icons/hud/shield_64x64.edds"` | `.edds` or `.paa` path | Shield icon displayed inside the `BUDDY INSURANCE POLICY` view. |
| `InsuranceHUDIconPath` | `string` | `"DayZStory_Buddy_Satellite/GUI/Icons/insurance.edds"` | `.edds` or `.paa` path | Badge icon displayed on the bottom-right player HUD. |
| `EditorJsonFiles` | `array<string>` | `[]` | List of `.json` filenames in `$profile:DayZStory/AiBuddy/DLC/Satellite/Terminals/` | **DayZ Editor `.json` Scene Import:** Automatically spawns all `DZS_SatelliteTerminal` objects and decorative `.p3d` props from exported DayZ Editor JSON files. (Tip: If you place an `eAI_SupplyCrate` near a terminal in DayZ Editor, the system automatically uses its exact position/rotation as that terminal's `ClaimBoxPosition` without spawning an empty dummy crate!). |

---

## 3. Radio Equipment Shop & Satellite Uplink Settings (`RadioShop`)

| Variable Name (Key) | Type | Default | Valid Values / Options | Description & Behavior |
| :--- | :---: | :---: | :--- | :--- |
| `RadioShop.Enable` | `bool` (`0`/`1`) | `1` | `0` = Disable Radio Shop<br>`1` = Enable Radio Shop | Whether players can purchase radios directly from the terminal. |
| `RadioShop.EconomyMode` | `int` | `-1` | `-1` = Inherit `m_EconomyMode` from `BuddySettings.json`<br>`0` = Hardline Reputation<br>`1` = eWallet Coins<br>`2` = Expansion Bank<br>`3` = Free | **Currency used to buy radios in the `RADIO EQUIPMENT SHOP`**. Can be configured independently from `InsuranceEconomyMode`. |
| `RadioShop.PairDuration` | `float` | `10.0` | `1.0` – `60.0` (seconds) | Duration of the progress bar and radio static loop when clicking **`PAIR RADIO`**. (Player must keep a powered-ON radio in hands during the entire uplink). |
| `RadioShop.RecallDuration` | `float` | `10.0` | `1.0` – `60.0` (seconds) | Duration of the progress bar and radio static loop when clicking **`RECALL BUDDY`** at the terminal. |
| `RadioShop.PairSuccessChance` | `float` | `0.85` | `0.01` – `1.0` (`0.85` = 85% chance, `1.0` = 100% guaranteed) | Probability that a **`PAIR RADIO`** attempt succeeds. On failure, an interference warning appears and the button changes to `TRY AGAIN`. |
| `RadioShop.RecallSuccessChance` | `float` | `0.85` | `0.01` – `1.0` (`0.85` = 85% chance, `1.0` = 100% guaranteed) | Probability that a **`RECALL BUDDY`** satellite uplink succeeds. On success, a notification chime plays and the button turns green (**`READY — CALL BUDDY`**). |

### Radio Shop Item Entries (`RadioShop.Items[]`)

| Field Name (Key) | Type | Example | Description & Behavior |
| :--- | :---: | :---: | :--- |
| `ClassName` | `string` | `"PersonalRadio"` | Item classname (must inherit from `TransmitterBase`). Rendered as a live 3D model inside the shop preview box. |
| `DisplayName` | `string` | `"Emergency Field Radio"` | Display name shown in the shop list and detail header. |
| `Price` | `int` | `1500` | Purchase price in `RadioShop.EconomyMode` currency. |
| `MaxStock` | `int` | `5` or `-1` | **Server-wide stock per server restart:**<br>• Positive integer (e.g. `5`): Displays `IN STOCK: 5` in the bottom-right corner of the 3D preview box, decrementing on each purchase until reaching `0` (red **`OUT OF STOCK`**).<br>• `-1`: Unlimited stock (`IN STOCK`). |
| `HealthMin` | `float` | `0.7` | Minimum randomized item health fraction (`0.05` – `1.0`, where `0.7` = 70% Worn). |
| `HealthMax` | `float` | `1.0` | Maximum randomized item health fraction (`0.05` – `1.0`, where `1.0` = 100% Pristine). |
| `Attachments` | `array<string>` | `["Battery9V"]` | Attachments spawned inside the purchased radio. Including `"Battery9V"` ensures players can immediately switch the radio ON and pair it at the terminal! |

---

## 4. Terminal Spawn Entries (`Terminals[]`)

| Field Name (Key) | Type | Example | Description & Behavior |
| :--- | :---: | :---: | :--- |
| `Name` | `string` | `"Safezone Terminal"` | Descriptive name logged on server startup. |
| `Position` | `[x, y, z]` | `[3705.5, 402.1, 5985.2]` | World coordinates (`Y` is automatically snapped to terrain `SurfaceY`). **Note:** Any entry with `[0.0, 0.0, 0.0]` is treated as an unconfigured placeholder and skipped. |
| `Orientation` | `[yaw, pitch, roll]` | `[180.0, 0.0, 0.0]` | Terminal rotation in degrees. |
| `ClassName` | `string` | `"DZS_SatelliteTerminal"` | Object classname to spawn (`"DZS_SatelliteTerminal"` or `"Land_Radio_PanelPAS"`). |
| `ClaimBoxPosition` | `[x, y, z]` | `[0.0, 0.0, 0.0]` | Exact position where the claimed **`eAI_SupplyCrate`** spawns. Leave at `[0.0, 0.0, 0.0]` to automatically spawn it next to the terminal. |
| `ClaimBoxOrientation` | `[yaw, pitch, roll]` | `[0.0, 0.0, 0.0]` | Rotation of the spawned `eAI_SupplyCrate`. |
| `RecallSpawnPosition` | `[x, y, z]` | `[0.0, 0.0, 0.0]` | Exact position where the companion spawns when clicking **`READY — CALL BUDDY`** at this terminal. Leave at `[0.0, 0.0, 0.0]` to spawn next to the player. |
