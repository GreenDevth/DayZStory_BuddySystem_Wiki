# 🇹🇭 DayZStory_BuddySystem — คู่มือผู้ดูแลเซิร์ฟเวอร์ฉบับสมบูรณ์ (Thai Wiki)

> **[🏠 กลับหน้าแรก (Main Wiki Index)](../README.md)** | **[🇺🇸 Switch to English Wiki](../en/README.md)**

เอกสารชุดนี้สร้างขึ้นเพื่อให้ผู้ดูแลเซิร์ฟเวอร์ (Server Admin) เข้าใจการทำงานของทุกตัวแปรใน **`DayZStory_BuddySystem`** อย่างแม่นยำ 100% โดยอ้างอิงจากโค้ดจริงของระบบ ไม่ต้องคาดเดาว่าแต่ละตัวแปรใส่ค่าอะไรได้บ้าง หรือตัวแปรไหนมีผลกระทบต่อกันอย่างไร

---

## 📚 สารบัญคู่มือภาษาไทย (Table of Contents)

1. **[บทที่ 1: สถาปัตยกรรมระบบและการติดตั้ง (01-Architecture-and-Setup.md)](./01-Architecture-and-Setup.md)**
   - แผนผังการทำงานของทั้ง **12 โมดูล (PBO)**
   - มอดที่ต้องติดตั้งควบคู่ (Required Dependencies)
   - โครงสร้างไฟล์ทั้งหมดในโฟลเดอร์ `$profile:DayZStory/AiBuddy/`
   - ระบบ **Self-Healing JSON** (การซ่อมและเติมตัวแปรใหม่อัตโนมัติ)

2. **[บทที่ 2: พจนานุกรมคอนฟิกหลัก `BuddySettings.json` (02-Core-BuddySettings.md)](./02-Core-BuddySettings.md)**
   - ตารางอธิบายตัวแปรทุกตัวใน `BuddySettings.json` พร้อมประเภทข้อมูล (Type), ค่าเริ่มต้น (Default), ตัวเลือกทั้งหมด (Valid Values) และเงื่อนไขการทำงาน
   - เจาะลึกการปรับแต่งบัดดี้แต่ละคนใน **`m_BuddyRoster`** ครบทั้ง 20 ตัวแปร (ความแม่นยำ, ระยะมองเห็น, การหมอบยิง Sniper Prone, ตัวคูณดาเมจ, หมัด Shoryuken, พฤติกรรมการเก็บของ)

3. **[บทที่ 3: ระบบตู้ดาวเทียม ประกันภัยดิจิทัล และร้านค้าวิทยุ (03-DLC-Satellite-and-Insurance.md)](./03-DLC-Satellite-and-Insurance.md)**
   - เจาะลึกไฟล์ `$profile:DayZStory/AiBuddy/DLC/Satellite/Setting.json`
   - การแยกสกุลเงินอิสระ 3 ส่วน (`RecoveryEconomyMode`, `InsuranceEconomyMode`, `RadioShop.EconomyMode`)
   - การตั้งค่าระยะเวลาและโอกาสสำเร็จของการจูนคลื่นวิทยุ (`PairDuration`, `PairSuccessChance`, `RecallDuration`, `RecallSuccessChance`)
   - ระบบสต็อกสินค้าในร้านค้าวิทยุ (`MaxStock`, `HealthMin`, `HealthMax`, `Attachments`)

4. **[บทที่ 4: คู่มือตั้งค่า DLC เสริมทั้งหมด (04-DLC-Gravedigger-Radio-Medic-HUD-Economy.md)](./04-DLC-Gravedigger-Radio-Medic-HUD-Economy.md)**
   - **`DLC/Contractors/Setting.json`**: จุดสปอน NPC นายหน้าว่าจ้างบัดดี้
   - **`DLC/Gravedigger/Setting.json`**: ระบบสัปเหร่อ หลุมศพ ค่ายืมพลั่ว และการโหลดฉากสุสานจากไฟล์ DayZ Editor (`.json`)
   - **`DLC/Radio/Setting.json`**: ระบบวิทยุพกพา การบังคับ Pair วิทยุ และการจำกัดคลาสวิทยุ (`AllowedRadioClasses`)
   - **`DLC/Medic/Setting.json`**: ระบบบัดดี้แพทย์สนามช่วยปั๊มหัวใจ (CPR / Epinephrine), พันแผล และดามขาหักอัตโนมัติ
   - **`DLC/HUD/Setting.json`**: การตั้งค่า 3D Marker บนหัวบัดดี้ ระบบเช็คสิ่งกีดขวางสายตา (LOS Raycast) และตำแหน่งไอคอนประกันภัยบนหน้าจอ
   - **`DLC/Economy/Setting.json`**: รางวัลแต้มเมื่อบัดดี้ช่วยฆ่าซอมบี้/สัตว์ป่า และระบบหักแต้มเมื่อยิงพวกเดียวกัน
   - **`DLC/Controller/Setting.json`**: การเลือกวงล้อคำสั่ง (`RadialMenuType`)
   - **โมดูลเสริมพิเศษ**: `DayZStory_Buddy_Admin`, `DayZStory_Buddy_HardlineOverride`, `Dog_Compatible`, `Horse_Compatible`

5. **[บทที่ 5: ตารางจำลองสถานการณ์การตายและการกู้คืน (05-Death-Insurance-and-Recovery-Matrix.md)](./05-Death-Insurance-and-Recovery-Matrix.md)**
   - ตารางเปรียบเทียบแบบชัดเจนว่า เมื่อ **ผู้เล่นตาย** หรือ **บัดดี้ตาย** ในแต่ละโหมด (`m_OwnerDeathBuddyMode: 0, 1, 2`) ร่วมกับการ **มีประกัน vs ไม่มีประกัน** จะเกิดอะไรขึ้นกับตัวบัดดี้ ศพของบัดดี้ และสัมภาระในกระเป๋า
   - เปรียบเทียบความแตกต่างระหว่างการกู้คืนผ่าน **ตู้ดาวเทียม (Satellite Terminal)** vs **การขุดหลุมศพกับสัปเหร่อ (Gravedigger Cemetery)**

6. **[บทที่ 6: ระบบ Faction, พันธมิตร และการวางพิกัด NPC (06-Factions-Coordinates-and-Editor-Guide.md)](./06-Factions-Coordinates-and-Editor-Guide.md)**
   - กลไกการซิงค์ **Faction** ของบัดดี้ให้ตรงกับผู้เล่นที่จ้างโดยอัตโนมัติ และการบันทึกลงไฟล์เซฟ
   - การตั้งค่า `FriendlyList` เพื่อไม่ให้บัดดี้หรือบอทเฝ้าเมืองยิงกันเอง
   - วิธีหาพิกัด `Position` / `Orientation` ในเกม, ตารางรหัสท่าทาง `EmoteID` และวิธีตั้งค่าไฟล์ `Expansion/Settings/AIPatrolSettings.json` / `Loadouts`

7. **[บทที่ 7: สูตรการตั้งค่าสำเร็จรูปและการแก้ปัญหา (07-Presets-and-Troubleshooting.md)](./07-Presets-and-Troubleshooting.md)**
   - **4 สูตรสำเร็จรูป (Copy-Paste Recipes)**: เซิร์ฟแนว PvE สายชิล, เซิร์ฟแนว PvPvE สมดุล, เซิร์ฟแนว Hardcore ขุดหลุมศพ และเซิร์ฟแยกหลายสกุลเงิน (Wallet + Hardline)
   - อธิบายโครงสร้างไฟล์เซฟผู้เล่น `$profile:DayZStory/AiBuddy/BuddyData/<steamid>.json` สำหรับแอดมินที่ต้องการตรวจสอบหรือแก้ไขข้อมูลผู้เล่นรายคน
   - คำถามที่พบบ่อยและวิธีแก้ปัญหา (Troubleshooting FAQ)
