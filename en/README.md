# 🇺🇸 DayZStory_BuddySystem — Official Server Admin Wiki (English)

> **[🏠 Back to Main Wiki Index](../README.md)** | **[🇹🇭 สลับเป็นคู่มือภาษาไทย (Thai Wiki)](../th/README.md)**

This documentation provides a complete, source-verified reference for every JSON configuration file, variable, enum option, cross-module condition, and server preset in **`DayZStory_BuddySystem`**.

---

## 📚 Table of Contents

1. **[Chapter 1: Architecture & Installation Guide (01-Architecture-and-Setup.md)](./01-Architecture-and-Setup.md)**
   - Overview of all **12 Modular PBOs** (3 Required Core + 9 Optional DLCs)
   - Required external dependencies (`DayZ Expansion`, `CF`, `DayZStory Frameworks`)
   - Complete `$profile:DayZStory/AiBuddy/` directory tree
   - Self-Healing JSON configuration system

2. **[Chapter 2: Core Configuration Reference `BuddySettings.json` (02-Core-BuddySettings.md)](./02-Core-BuddySettings.md)**
   - Full dictionary of every root variable in `BuddySettings.json` (Type, Default, Valid Values, Conditions)
   - Economy Modes (`m_EconomyMode`: Hardline Reputation, eWallet Coins, Expansion Bank, Free)
   - Owner Death Modes (`m_OwnerDeathBuddyMode`: Mode 0 Satellite/Cemetery Lock, Mode 1 Roaming AI, Mode 2 Safe Despawn & Recallable)
   - Complete 20-field dictionary for **`m_BuddyRoster`** (`BuddyEntryConfig`: Accuracy, Threat Range, Sniper Prone Threshold, Damage Multipliers, Shoryuken Punch, Looting Behavior)

3. **[Chapter 3: Satellite Terminal, Digital Insurance & Radio Shop (03-DLC-Satellite-and-Insurance.md)](./03-DLC-Satellite-and-Insurance.md)**
   - Complete reference for `$profile:DayZStory/AiBuddy/DLC/Satellite/Setting.json`
   - Independent Multi-Currency overrides (`RecoveryEconomyMode`, `InsuranceEconomyMode`, `RadioShop.EconomyMode`)
   - Radio Pairing & Satellite Recall duration and success chance (`PairDuration`, `PairSuccessChance`, `RecallDuration`, `RecallSuccessChance`)
   - Radio Equipment Shop stock system (`MaxStock`, `HealthMin`, `HealthMax`, `Attachments`) and Terminal placement (`ClaimBoxPosition`, `RecallSpawnPosition`)

4. **[Chapter 4: Optional DLC Configuration Reference (04-DLC-Gravedigger-Radio-Medic-HUD-Economy.md)](./04-DLC-Gravedigger-Radio-Medic-HUD-Economy.md)**
   - **`DLC/Contractors/Setting.json`**: Contractor NPC spawns, loadouts, passive mode, and idle emotes
   - **`DLC/Gravedigger/Setting.json`**: Cemetery Gravedigger NPCs, borrowed shovel (`DZS_GravediggerShovel`), grave digging (`DZS_BuddyGrave`), and **DayZ Editor `.json` scene imports**
   - **`DLC/Radio/Setting.json`**: Portable handheld radio controls, mandatory pairing, and allowed radio classes
   - **`DLC/Medic/Setting.json`**: Realtime AI Field Medic assistance (CPR / Epinephrine revival, bleeding bandaging, broken leg splinting)
   - **`DLC/HUD/Setting.json`**: 3D overhead markers, physics Line-of-Sight (LOS) raycasting, and HUD insurance badge coordinates
   - **`DLC/Economy/Setting.json`**: Kill rewards (zombies/wildlife/AI) and friendly-fire/betrayal penalties
   - **`DLC/Controller/Setting.json`**, **`DayZStory_Buddy_Admin` (`cfggameplay.json`)**, **`HardlineOverride`**, and **`Dog/Horse Compatible`**

5. **[Chapter 5: Death, Insurance & Gear Recovery Scenario Matrix (05-Death-Insurance-and-Recovery-Matrix.md)](./05-Death-Insurance-and-Recovery-Matrix.md)**
   - Complete truth-table matrix comparing what happens when the **Buddy dies** vs. when the **Player (Owner) dies** across all modes, with or without Digital Insurance
   - Side-by-side comparison of **Satellite Terminal Recovery** vs. **Gravedigger Cemetery Excavation**

6. **[Chapter 6: Faction Sync, FriendlyList, Coordinates & DayZ Editor Guide (06-Factions-Coordinates-and-Editor-Guide.md)](./06-Factions-Coordinates-and-Editor-Guide.md)**
   - Automatic Owner-to-Buddy Faction synchronization and persistence
   - Configuring `FriendlyList` to prevent friendly fire with custom server NPCs
   - Exact 3D positioning rules, popular `EmoteID` table, and importing cemetery scenes from DayZ Editor

7. **[Chapter 7: Server Presets (Recipes) & Troubleshooting Guide (07-Presets-and-Troubleshooting.md)](./07-Presets-and-Troubleshooting.md)**
   - **4 Copy-Paste Server Presets**: PvE Casual, PvPvE Tactical Standard, Hardcore Cemetery Survival, and Multi-Currency Economy
   - Inspecting and editing player save files (`$profile:DayZStory/AiBuddy/BuddyData/<steamid>.json`)
   - Troubleshooting FAQ
