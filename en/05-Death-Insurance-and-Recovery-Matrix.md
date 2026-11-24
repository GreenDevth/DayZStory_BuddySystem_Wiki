# Chapter 5: Death, Insurance & Gear Recovery Scenario Matrix

> **[⬅️ Chapter 4: Optional DLC Configuration Reference](./04-DLC-Gravedigger-Radio-Medic-HUD-Economy.md)** | **[➡️ Chapter 6: Factions, Coordinates & DayZ Editor Guide](./06-Factions-Coordinates-and-Editor-Guide.md)**

This chapter provides a complete truth-table matrix explaining what happens to the companion entity, corpse, and inventory across every death and recovery configuration.

---

## 1. Case 1: When the **Companion AI (Buddy)** is Killed in Action

| `m_EnableRecoverySystem` | Player's Active Digital Insurance (`InsuranceCount`) | Is `Gravedigger` DLC Enabled? | What Happens to the Companion's Corpse? | What Happens to the Save File & How to Recover Gear? |
| :---: | :---: | :---: | :--- | :--- |
| **`1` (Enabled)** | **Insured (`>= 1` Policy)** | Yes or No | All inventory items are saved to `<steamid>.json` and **immediately wiped from the corpse** to prevent double-looting (100% Anti-Dupe). | • Save file is locked (`OwnerDeadState = 1, RemoveReason = 2`).<br>• Player visits a **Satellite Terminal** and claims all gear in a private **`eAI_SupplyCrate`** (consumes 1 policy + recovery fee).<br>• Or excavates a grave at the **Gravedigger Cemetery** to receive the `eAI_SupplyCrate`. |
| **`1` (Enabled)** | **Uninsured (`0` Policies)** | **No (`Gravedigger` Disabled)** | **All items remain on the companion's corpse naturally.** Anyone can loot the body in the field. | • **Save file is permanently deleted immediately (Permadeath).**<br>• Purchasing insurance *after* the companion dies will not recover a companion who died uninsured. |
| **`1` or `0`** | **Uninsured (`0` Policies)** | **Yes (`Gravedigger` Enabled)** | All items are transferred to cemetery storage and wiped from the corpse (Anti-Dupe). | • Cannot be claimed via Satellite Terminal without insurance.<br>• However, the player can travel to the **Gravedigger Cemetery**, rent a shovel (`DZS_GravediggerShovel`), and **dig up a grave (`DZS_BuddyGrave`)** to recover the `eAI_SupplyCrate`! |

---

## 2. Case 2: When the **Player (Owner)** Dies (While Companion is Still Alive)

Controlled by **`m_OwnerDeathBuddyMode`** in `BuddySettings.json`:

| `m_OwnerDeathBuddyMode` | Mode Name | What Happens to the Companion Immediately on Owner Death? | What Can the Player Do After Respawning? | Recommended Server Style |
| :---: | :--- | :--- | :--- | :--- |
| **`0`** *(Default)* | **Satellite / Cemetery Lock** | Companion's inventory is saved and the companion is **safely despawned immediately**. The save file is locked (`OwnerDeadState = 1, RemoveReason = 1`). Cannot be recalled via radio. | • **Option A (Satellite Terminal):** Visit a Satellite Terminal (you can purchase a Digital Insurance policy right there if you don't have one) and claim all companion gear in a private **`eAI_SupplyCrate`**.<br>• **Option B (Gravedigger Cemetery):** Rent a shovel and dig up a grave to receive the **`eAI_SupplyCrate`**.<br>• *(If the player visits a Contractor to hire a new companion without claiming their old gear first, a warning dialog `UNRECOVERED COMPANION GEAR` asks for confirmation before wiping the old save).* | **PvPvE / Tactical Survival** (Meaningful death penalty: must pay recovery fee and hire a new companion, while protecting stored companion gear). |
| **`1`** | **Release as Roaming AI** | Companion is removed from the player's group, becomes an independent roaming AI (`eAIFactionRoamingBuddy`) at the death site, and the **save file is deleted immediately**. | • Cannot be claimed at Satellite Terminals or Graves.<br>• Must hire a new companion at a Contractor NPC. | **Hardcore Survival** (Owner death terminates the companion contract immediately). |
| **`2`** | **Safe Despawn & Recallable** | Companion's inventory is saved and the companion is **safely despawned without locking** (`OwnerDeadState = 0, RemoveReason = 0`). | • Visit a **Satellite Terminal** (or use a paired handheld radio) and click **`RECALL BUDDY`**.<br>• **Your exact same companion spawns back with all their gear intact!** No crate claiming or re-hiring required. | **PvE Casual / Solo-Friendly** (Owner death does not forfeit your companion). |

---

## 3. Side-by-Side Comparison: Satellite Terminal vs. Gravedigger Cemetery

You can enable **either one** or **both simultaneously**:

| Feature | 📡 Satellite Terminal (`DayZStory_Buddy_Satellite`) | ⚰️ Gravedigger Cemetery (`DayZStory_Buddy_Gravedigger`) |
| :--- | :--- | :--- |
| **Theme & Immersion** | High-Tech / Military Uplink Terminal in Safezones or Outposts | Gritty Survival / Church Cemetery with Gravedigger NPC & Shovel |
| **Requires Digital Insurance (`InsuranceCount >= 1`)?** | **Yes** (Must hold `>= 1` policy before buddy dies; for owner death, can be bought at the terminal before claiming). | **No Digital Insurance Required** (Pay the shovel rental fee `ShovelBorrowCost` + excavation fee `RecoveryReputationCost`). |
| **Player Workflow** | Open Terminal UI -> `BUDDY INSURANCE POLICY` -> Click Claim -> `eAI_SupplyCrate` spawns next to terminal. | Talk to Gravedigger NPC (`Borrow Shovel`) -> Walk to `DZS_BuddyGrave` -> Hold Left-Click for 6s to dig -> `eAI_SupplyCrate` spawns on grave. |
| **Anti-Dupe & Cleanup** | Save file is wiped immediately when the crate spawns; crate can be dismantled with a Screwdriver/Crowbar when emptied. | Identical `eAI_SupplyCrate` behavior; borrowed shovel is auto-confiscated if carried outside `CemeteryRadius`. |
