# Chapter 7: Server Presets (Recipes) & Troubleshooting Guide

> **[⬅️ Chapter 6: Factions, Coordinates & DayZ Editor Guide](./06-Factions-Coordinates-and-Editor-Guide.md)** | **[🏠 Back to English Wiki Home](./README.md)**

---

## 1. Four Copy-Paste Server Presets (Recipes)

### 🟢 Preset 1: PvE Casual / Solo-Friendly (No Companion Loss on Player Death)
- **Behavior:** When the player dies, their companion safely despawns without locking (`m_OwnerDeathBuddyMode: 2`). After respawning, the player can immediately recall their exact same companion with all gear intact. Handheld radios work out-of-the-box without requiring Satellite Terminal pairing.
- **`BuddySettings.json`:**
  ```json
  "m_EconomyMode": 0,
  "m_DeleteAIOnPlayerDisconnect": 1,
  "m_DeleteAIOnPlayerDeath": 1,
  "m_OwnerDeathBuddyMode": 2,
  "m_EnableRecoverySystem": 0
  ```
- **`DLC/Radio/Setting.json`:**
  ```json
  "Enable": 1,
  "EnableCallConstructorsMenu": 0,
  "EnableDirectRadioRecall": 1,
  "EnableCallBackCost": 0,
  "RequireRadioPairing": 0
  ```

---

### 🟡 Preset 2: PvPvE Tactical Standard (Recommended Default)
- **Behavior:** Players must pair their handheld radio at a Satellite Terminal once per life (`RequireRadioPairing: 1`) and purchase **Digital Insurance** to protect companion gear. When the companion or owner dies, gear is claimed in a private **`eAI_SupplyCrate`** at a Satellite Terminal.
- **`BuddySettings.json`:**
  ```json
  "m_EconomyMode": 0,
  "m_DeleteAIOnPlayerDisconnect": 1,
  "m_DeleteAIOnPlayerDeath": 1,
  "m_OwnerDeathBuddyMode": 0,
  "m_EnableRecoverySystem": 1
  ```
- **`DLC/Radio/Setting.json`:**
  ```json
  "Enable": 1,
  "EnableDirectRadioRecall": 1,
  "RequireRadioPairing": 1
  ```

---

### 🔴 Preset 3: Hardcore Survival & Cemetery Gravedigger (Grave Excavation Only)
- **Behavior:** Disables field radio recall (`EnableDirectRadioRecall: 0`) so players must recall companions at a Satellite Terminal, and disables digital terminal crate claiming (`m_EnableRecoverySystem: 0`) so players must travel to the **Gravedigger Cemetery**, rent a shovel, and physically dig up a grave (`DZS_BuddyGrave`) to recover fallen companion gear!
- **`BuddySettings.json`:**
  ```json
  "m_OwnerDeathBuddyMode": 0,
  "m_EnableRecoverySystem": 0
  ```
- **`DLC/Radio/Setting.json`:**
  ```json
  "Enable": 1,
  "EnableDirectRadioRecall": 0,
  "RequireRadioPairing": 1
  ```
- **`DLC/Gravedigger/Setting.json`:**
  ```json
  "Enable": 1,
  "RecoveryReputationCost": 1500,
  "ShovelBorrowCost": 250
  ```

---

### 🟣 Preset 4: Multi-Currency Economy (Hardline Reputation + eWallet Coins)
- **Behavior:** Uses **Hardline Reputation (`0`)** for hiring companions and purchasing radios, while using **eWallet Coins (`1`)** (or Expansion Bank `2`) for Digital Insurance policies and gear recovery fees. The Satellite Terminal Main Hub automatically displays both currencies side-by-side (`48520 Rep  |  15000 Coins`).
- **`DLC/Satellite/Setting.json`:**
  ```json
  "RecoveryReputationCost": 5000,
  "RecoveryEconomyMode": 1,
  "InsuranceCost": 10000,
  "InsuranceEconomyMode": 1,
  "MaxInsurancePolicies": 3,
  "RadioShop": {
    "Enable": 1,
    "EconomyMode": 0
  }
  ```

---

## 2. Inspecting Player Save Files (`BuddyData/<SteamID64>.json`)

Path: `$profile:\DayZStory\AiBuddy\BuddyData\<SteamID64>.json`

| Save Field | Values | Meaning & Admin Intervention |
| :--- | :---: | :--- |
| `OwnerDeadState` | `0` or `1` | `0` = Normal (companion can be recalled).<br>`1` = **Locked awaiting gear recovery** (set to `0` if you want to manually unlock a player's companion so they can recall them without claiming a crate). |
| `RemoveReason` | `0`, `1`, `2`, `3` | `0` = Normal disconnect/restart, `1` = Owner died, `2` = Companion died (insured), `3` = Dismissed. |
| `RadioPaired` | `0` or `1` | `1` = Player has paired their radio with a Satellite Terminal during this life. |
| `Faction` | e.g. `"eAIFactionCivilian"` | Persisted `eAIFaction` class synchronized from the owner. |

---

## 3. Troubleshooting FAQ

1. **Why is the `PAIR RADIO` or `RECALL BUDDY` button disabled (`HOLD POWERED RADIO`) at the Satellite Terminal?**
   - The player must hold a radio **in their hands** (`In Hands` slot), and the radio must have a **9V Battery attached and be Switched ON**.
2. **Why didn't my Satellite Terminal or Gravedigger NPC spawn?**
   - Check that `"Position"` is not `[0.0, 0.0, 0.0]` (placeholder entries at `0,0,0` are skipped) and validate your JSON syntax using PowerShell:
     `Get-Content "Setting.json" -Raw | ConvertFrom-Json`
3. **Why isn't my companion performing CPR or bandaging me?**
   - Check `DLC/Medic/Setting.json`: If `"OnlyMedicRole": 1`, only companions with `"Medic"` in their `m_Role` will perform medical rescues. If `"RequireItems": 1`, the companion must carry bandages or a splint inside **their own inventory** (CPR is always free within cooldown).
