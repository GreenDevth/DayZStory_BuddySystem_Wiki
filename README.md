# DayZStory_BuddySystem — Official Server Admin & Developer Wiki

> **[🇹🇭 คู่มือภาษาไทย (Thai Wiki)](./th/README.md)** | **[🇺🇸 English Wiki (Documentation)](./en/README.md)**
> **Steam Workshop:** [DayZStory_BuddySystem (ID: 3844559966)](https://steamcommunity.com/sharedfiles/filedetails/?id=3844559966)

---

## 🇹🇭 สารบัญภาษาไทย (Thai Documentation)

ยินดีต้อนรับสู่คู่มือผู้ดูแลเซิร์ฟเวอร์ (Server Admin Wiki) ฉบับสมบูรณ์สำหรับ **`DayZStory_BuddySystem`** เอกสารชุดนี้เจาะลึกทุกตัวแปรในไฟล์ `.json` ทุกทางเลือก (Enum/Values) เงื่อนไขการทำงานร่วมกันระหว่างโมดูล และสูตรการตั้งค่าสำเร็จรูปตามแนวทางเซิร์ฟเวอร์ของคุณ

| บทที่ | หัวข้อเอกสาร | รายละเอียดเนื้อหา |
| :---: | :--- | :--- |
| **00** | **[หน้าหลักสารบัญไทย (Thai Home)](./th/README.md)** | ภาพรวมระบบ โครงสร้างโฟลเดอร์ `$profile` และแผนผังการเชื่อมโยงโมดูล |
| **01** | **[สถาปัตยกรรมและการติดตั้ง (Architecture & Setup)](./th/01-Architecture-and-Setup.md)** | รายชื่อทั้ง 12 PBO (Core, GUI, Controller, Optional DLCs), มอดที่ต้องลงก่อน (Dependencies) และโครงสร้างไฟล์ใน `$profile:DayZStory/AiBuddy/` |
| **02** | **[พจนานุกรมคอนฟิกหลัก (Core BuddySettings.json)](./th/02-Core-BuddySettings.md)** | เจาะลึกทุกตัวแปรใน `BuddySettings.json`, โหมดสกุลเงิน (`m_EconomyMode`), โหมดการตาย (`m_OwnerDeathBuddyMode`), และการปรับแต่งความเก่งของบัดดี้รายตัว (`m_BuddyRoster` ครบทั้ง 20 ฟิลด์) |
| **03** | **[ระบบดาวเทียม ประกันภัย และร้านค้าวิทยุ (Satellite, Insurance & Radio Shop)](./th/03-DLC-Satellite-and-Insurance.md)** | คู่มือ `DLC/Satellite/Setting.json`, การแยกสกุลเงิน 3 ส่วน (กู้ของ / ซื้อประกัน / ซื้อวิทยุ), ระบบ Pair & Recall วิทยุ, สต็อกร้านค้าวิทยุ และการวางพิกัดตู้ดาวเทียม |
| **04** | **[ระบบสัปเหร่อ หลุมศพ และโมดูลเสริม (Gravedigger, Radio, Medic, HUD, Economy)](./th/04-DLC-Gravedigger-Radio-Medic-HUD-Economy.md)** | คู่มือ `Setting.json` ของ DLC ทั้งหมด: สัปเหร่อ (`Gravedigger` + `DayZ Editor JSON`), วิทยุพกพา (`Radio`), บัดดี้แพทย์สนาม (`Medic`), 3D Marker (`HUD`), เศรษฐกิจ (`Economy`), วงล้อคำสั่ง (`Controller`), สิทธิ์แอดมิน (`Admin`) และ `HardlineOverride` |
| **05** | **[ตารางจำลองสถานการณ์การตายและการกู้คืน (Death, Insurance & Recovery Matrix)](./th/05-Death-Insurance-and-Recovery-Matrix.md)** | ตารางเปรียบเทียบทุกกรณี: เมื่อผู้เล่นตาย vs บัดดี้ตาย ในโหมด `0, 1, 2` + มีประกัน/ไม่มีประกัน + เปิดตู้ดาวเทียมหรือสัปเหร่อ จะเกิดอะไรขึ้นกับตัวบัดดี้และสัมภาระ |
| **06** | **[ระบบ Faction, พันธมิตร และการวางพิกัด NPC (Factions, Coordinates & Editor Guide)](./th/06-Factions-Coordinates-and-Editor-Guide.md)** | การซิงค์ Faction บัดดี้ตามผู้เล่นอัตโนมัติ, การตั้งค่า `FriendlyList`, วิธีหาพิกัด `Position`/`Orientation`, ตาราง `EmoteID` ยอดนิยม และการนำฉากจาก DayZ Editor มาใช้ |
| **07** | **[สูตรการตั้งค่าสำเร็จรูปและการแก้ปัญหา (Server Presets & Troubleshooting)](./th/07-Presets-and-Troubleshooting.md)** | สูตร JSON พร้อมใช้ 4 สไตล์ (PvE Casual, PvPvE Tactical, Hardcore Cemetery, Multi-Currency Server) พร้อมวิธีตรวจสอบไฟล์เซฟ `BuddyData/<steamid>.json` และแก้ปัญหาที่พบบ่อย |

---

## 🇺🇸 English Documentation Index

Welcome to the complete Server Administrator Wiki for **`DayZStory_BuddySystem`**. This documentation covers every JSON configuration variable, valid enum values, cross-module conditions, and ready-to-use server presets.

| Chapter | Document Title | Description |
| :---: | :--- | :--- |
| **00** | **[English Wiki Home](./en/README.md)** | System overview, `$profile` directory tree, and module architecture |
| **01** | **[Architecture & Installation Guide](./en/01-Architecture-and-Setup.md)** | Full breakdown of all 12 PBOs (Core, GUI, Controller, Optional DLCs), required dependencies, and `$profile:DayZStory/AiBuddy/` file structure |
| **02** | **[Core Configuration (`BuddySettings.json`)](./en/02-Core-BuddySettings.md)** | Complete reference for `BuddySettings.json`, economy modes (`m_EconomyMode`), death modes (`m_OwnerDeathBuddyMode`), and per-buddy AI stats (`m_BuddyRoster` — all 20 fields) |
| **03** | **[Satellite Terminal, Digital Insurance & Radio Shop](./en/03-DLC-Satellite-and-Insurance.md)** | Reference for `DLC/Satellite/Setting.json`, independent currency overrides (Recovery / Insurance / Radio Shop), Radio Pairing & Recall probabilities, stock system, and terminal placement |
| **04** | **[Gravedigger, Radio, Medic, HUD, Economy & Other DLCs](./en/04-DLC-Gravedigger-Radio-Medic-HUD-Economy.md)** | Reference for `Gravedigger` (including DayZ Editor `.json` graves), `Radio`, `Medic`, `HUD`, `Economy`, `Controller`, `Admin` (`cfggameplay.json`), `HardlineOverride`, and Animal Compatibility PBOs |
| **05** | **[Death, Insurance & Gear Recovery Scenario Matrix](./en/05-Death-Insurance-and-Recovery-Matrix.md)** | Truth-table matrix explaining exactly what happens to the buddy and their inventory when the Owner dies vs. when the Buddy dies across all modes |
| **06** | **[Faction Sync, FriendlyList, Coordinates & DayZ Editor Guide](./en/06-Factions-Coordinates-and-Editor-Guide.md)** | How automatic Owner-to-Buddy Faction synchronization works, configuring `FriendlyList`, finding `Position`/`Orientation`, popular `EmoteID`s, and importing DayZ Editor scenes |
| **07** | **[Server Presets (Recipes) & Troubleshooting Guide](./en/07-Presets-and-Troubleshooting.md)** | 4 copy-paste configuration recipes (PvE Casual, PvPvE Tactical, Hardcore Gravedigger, Dual-Currency Economy), save file inspection (`BuddyData/<steamid>.json`), and FAQ |
