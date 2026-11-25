# บทที่ 6: ระบบ Faction, พันธมิตร และการวางพิกัด NPC (Factions, Coordinates & DayZ Editor Guide)

> **[⬅️ บทที่ 5: ตารางจำลองสถานการณ์การตายและการกู้คืน](./05-Death-Insurance-and-Recovery-Matrix.md)** | **[➡️ บทที่ 7: สูตรการตั้งค่าสำเร็จรูปและการแก้ปัญหา](./07-Presets-and-Troubleshooting.md)**

---

## 1. ระบบซิงค์ Faction บัดดี้ตามผู้เล่นอัตโนมัติ (Owner-to-Buddy Faction Synchronization)

ในระบบ של **DayZ Expansion AI** ตัวแปร **Faction** ของกลุ่ม AI (`eAIGroup`) เป็นตัวกำหนดว่า AI ตัวนั้นจะเป็นมิตร (Friendly) หรือเป็นศัตรู (Hostile) กับผู้เล่นและบอทกลุ่มอื่น

### ลำดับการค้นหาและกำหนด Faction ของบัดดี้ (`BuddyPlugins.c`):
เมื่อผู้เล่นกดจ้างบัดดี้ (`Hire`), กดรับเข้ากลุ่ม (`Recruit`), หรือกดเรียกบัดดี้กลับมาจากเซฟ (`Recall`) ระบบจะตรวจสอบและกำหนด Faction ให้บัดดี้ตามลำดับดังนี้:

1. **ลำดับที่ 1 — ดึงจากระบบ `DayZStory_PlayerFaction` ของเจ้าของ (หากเซิร์ฟเวอร์มีมอดนี้):**
   - ระบบจะอ่านไฟล์ `$profile:DayZStory/PlayerFaction/PlayerData/<SteamID>.json` ของผู้เล่นที่จ้าง
   - แปลงชื่อสายอาชีพ/ฝ่ายของผู้เล่นให้เป็นคลาส `eAIFaction` ที่ตรงกันอัตโนมัติ เช่น:
     - `"Military"` -> `"eAIFactionRoleMilitary"`
     - `"Medic"` -> `"eAIFactionRoleMedic"`
     - `"Police"` -> `"eAIFactionRolePolice"`
     - `"Bandit"` / `"Raider"` -> `"eAIFactionRaiders"`
     - `"Mercenary"` -> `"eAIFactionMercenaries"`
     - `"Survivor"` / `"Civilian"` -> `"eAIFactionCivilian"` (หรือคลาส Faction ที่ระบุไว้โดยตรง)
2. **ลำดับที่ 2 — ดึงจาก `eAIGroup` ปัจจุบันของผู้เล่น:**
   - หากผู้เล่นอยู่ในกลุ่มที่มี Faction เฉพาะอยู่แล้ว (ที่ไม่ใช่ `eAIFactionPlayerSurvivor` พื้นฐาน) บัดดี้จะใช้ Faction เดียวกับกลุ่มของผู้เล่นทันที
3. **ลำดับที่ 3 — ดึงจากค่า `"Faction"` ที่บันทึกไว้ในไฟล์เซฟ `<SteamID>.json`:**
   - ทุกครั้งที่บัดดี้ถูกเซฟ ระบบจะบันทึกชื่อคลาส Faction ลงในฟิลด์ `"Faction"` ของไฟล์ `$profile:DayZStory/AiBuddy/BuddyData/<SteamID>.json` เช่น:
     ```json
     {
         "ClassName": "eAI_SurvivorF_Linda",
         "BuddyName": "Sophia",
         "ProfileID": "linda_allrounder",
         "Faction": "eAIFactionCivilian",
         "OwnerDeadState": 0
     }
     ```
4. **ลำดับที่ 4 — ค่าเริ่มต้นมาตรฐาน:**
   - หากไม่ตรงกับข้อ 1-3 ระบบจะใช้ `"eAIFactionCivilian"` ซึ่งเป็นมิตรกับผู้เล่นและ NPC ในเซฟโซน

---

## 2. การตั้งค่า `FriendlyList` ใน `BuddySettings.json`

ในไฟล์ `$profile:DayZStory/AiBuddy/BuddySettings.json` มีอาร์เรย์ `"FriendlyList"`:

```json
"FriendlyList": [
    "eAIFactionPlayerSurvivor",
    "eAIFactionCivilian",
    "eAIFactionFriendlyAll",
    "eAIFactionRoamingBuddy",
    "eAIFactionContractors",
    "eAIFactionGravediggers",
    "eAIFactionQuestGivers",
    "eAIFactionGuildMaster",
    "eAIFactionQuestReceivers",
    "eAIFactionObservers",
    "eAIFactionGuards"
]
```

### `FriendlyList` ทำงานอย่างไร?
- รายชื่อคลาส Faction ทุกตัวที่อยู่ใน `"FriendlyList"` จะถูกบันทึกว่าเป็น **พันธมิตร (Friendly)** กับทั้ง:
  1. **NPC นายหน้าว่าจ้างบัดดี้ (`eAIFactionContractors`)**
  2. **NPC สัปเหร่อ (`eAIFactionGravediggers`)**
  3. **บัดดี้ที่ถูกปล่อยพเนจร (`eAIFactionRoamingBuddy`)**
- **เมื่อใดที่แอดมินควรเพิ่มชื่อลงใน `FriendlyList`?**
  - หากเซิร์ฟเวอร์ของคุณมีการสร้างคลาส Faction ใหม่สำหรับบอทเฝ้าเซฟโซน หรือบอทเควสต์จากมอดอื่น แล้วไม่อยากให้ NPC นายหน้า/สัปเหร่อหันไปยิงบอทเหล่านั้น ให้เพิ่มชื่อคลาส `eAIFaction...` นั้นลงใน `"FriendlyList"` ได้เลยครับ

> [!WARNING]
> **ข้อควรระวังเรื่องการยิงพวกเดียวกันต่อหน้า NPC (Friendly-Fire Retaliation):**
> แม้ Faction ของผู้เล่นจะอยู่ใน `FriendlyList` แต่ระบบของ DayZ Expansion AI มีกลไกป้องกันตัว: **หากผู้เล่นยิงทำร้ายบัดดี้ของตนเอง หรือยิงผู้เล่นอื่นในระยะสายตาของ NPC Contractor / Gravedigger** (เมื่อตั้ง `"IsPassive": 0`) NPC จะถือว่าผู้เล่นคนนั้นเป็นภัยคุกคามและยิงสวนทันที!
> หากวาง NPC ไว้ใน Safezone และไม่ต้องการให้ NPC ชักปืนยิงใครไม่ว่ากรณีใดๆ ให้ตั้ง **`"IsPassive": 1`** ใน `DLC/Contractors/Setting.json` และ `DLC/Gravedigger/Setting.json` ครับ

---

## 3. วิธีหาพิกัด `Position` และมุมหัน `Orientation` สำหรับวาง NPC และตู้ดาวเทียม

เมื่อต้องการวาง **NPC Contractor**, **NPC Gravedigger**, **หลุมศพ (`DZS_BuddyGrave`)** หรือ **ตู้ดาวเทียม (`DZS_SatelliteTerminal`)**:

### รูปแบบพิกัดในไฟล์ JSON:
```json
"Position": [3705.42, 402.15, 5985.10],
"Orientation": [-45.0, 0.0, 0.0]
```
- **`Position`**: `[X (ตะวันตก-ตะวันออก), Y (ความสูงจากระดับน้ำทะเล), Z (ใต้-เหนือ)]`
  - **ความแตกต่างสำคัญ:**
    - สำหรับ **NPC Contractor, NPC Gravedigger และหลุมศพ**: ระบบใช้ค่า `Y` (ความสูง) ตามที่คุณกรอก **ตรงๆ 100%** (ไม่ได้บังคับดึงลงพื้นดิน `SurfaceY`) เพื่อให้คุณสามารถวาง NPC ยืนบนระเบียงบ้าน ชั้น 2 ของตึก หรือบนพื้นไม้ในค่ายได้โดยที่ขาไม่จมพื้น! ดังนั้นต้องใส่ค่า `Y` ให้ตรงกับระดับพื้นที่ยืนจริงเสมอ
    - สำหรับ **ตู้ดาวเทียม (`Terminals`)**: ระบบจะดึงความสูงพื้นดิน `SurfaceY(X, Z)` ให้อัตโนมัติ (แต่หากวางไว้ในแมพผ่าน DayZ Editor แล้ว ผู้เล่นก็สามารถกดใช้งานตู้ที่วางจาก DayZ Editor ได้เช่นกัน เพราะระบบผูก Action ไว้กับตัวคลาส `DZS_SatelliteTerminal` โดยตรง!)
  - **กฎเหล็ก `[0.0, 0.0, 0.0]`:** หากรายการใดมี `"Position": [0.0, 0.0, 0.0]` ระบบจะถือว่าเป็นเพียงแม่แบบ (Template Placeholder) และ **จะไม่เสกวัตถุนั้นออกมา**
- **`Orientation`**: `[Yaw (มุมหันซ้าย-ขวา -180 ถึง 180 หรือ 0 ถึง 360), Pitch (ก้ม-เงย ควรเป็น 0.0), Roll (เอียงซ้าย-ขวา ควรเป็น 0.0)]`

---

## 4. ตารางรหัสท่าทาง `EmoteID` ยอดนิยมสำหรับ NPC Contractor และ Gravedigger

ใน `DLC/Contractors/Setting.json` และ `DLC/Gravedigger/Setting.json` คุณสามารถกำหนด `"EmoteID"` เพื่อให้ NPC แสดงท่าทางเมื่อยืนว่างได้ (ระบบจะหน่วงเวลา 15 วินาทีหลังเสกเพื่อให้ NPC สวมชุดและสะพายปืนเสร็จก่อน แล้วจึงเริ่มทำท่าทาง):

| ค่า `EmoteID` | ค่าคงที่ใน DayZ (`DayZPlayerConstants`) | ลักษณะท่าทางของ NPC |
| :---: | :--- | :--- |
| **`0`** | *(ไม่มี - ค่าเริ่มต้น)* | ยืนถือปืน/ยืนประจำจุดตามปกติ |
| **`10`** | `ID_EMOTE_CAMPFIRE` | นั่งขัดสมาธิผิงไฟกับพื้น |
| **`11`** | `ID_EMOTE_SITA` | นั่งพักบนพื้น (ท่าที่ 1) |
| **`12`** | `ID_EMOTE_SITB` | นั่งพักชันเข่าบนพื้น (ท่าที่ 2) |
| **`1`** | `ID_EMOTE_GREETING` | โบกมือทักทาย |
| **`7`** | `ID_EMOTE_TAUNTKISS` | ส่งจูบ |
| **`9`** | `ID_EMOTE_TAUNTELBOW` | กอดอก / ท่าทางท้าทาย |
| **`44`** | `ID_EMOTE_SALUTE` | วันทยหัตถ์ (ตะเบ๊ะแบบทหาร) |
| **`58`** | `ID_EMOTE_LISTENING` | เอามือป้องหูตั้งใจฟัง |

---

## 5. วิธีนำไฟล์ฉากจาก DayZ Editor (`"EditorJsonFiles": []`) มาใช้กำหนดทั้ง Props, Target และ NPC (ครบทั้ง 3 โมดูล)

ทั้ง 3 โมดูลที่มีการเสกวัตถุในโลกเกม (**Contractors**, **Gravedigger**, **Satellite**) รองรับตัวแปร `"EditorJsonFiles": []` ในไฟล์ `Setting.json` ของแต่ละโมดูลเหมือนกันทั้งหมด:

- **หากไม่ได้ใส่ไฟล์ (`"EditorJsonFiles": []` ว่างเปล่า):** ระบบจะโหลดค่าเริ่มต้นและเสกเฉพาะรายการที่ระบุพิกัด `Position` ไว้ใน `Setting.json` ตามปกติ (และหากรายการใดตั้ง `"Position": [0.0, 0.0, 0.0]` ระบบจะข้ามไม่เสกจุดนั้น)
- **หากใส่ชื่อไฟล์ใน `"EditorJsonFiles": ["my_scene.json"]`:** ระบบจะอ่านไฟล์ `.json` ที่ Export จาก **DayZ Editor** โดยไม่ได้เสกแค่ Props ประกอบฉากเท่านั้น แต่ยังตรวจจับ **ClassName (`"name"`)** เพื่อตั้งค่าเป็น **NPC** และ **Target (จุดกดโต้ตอบ)** ให้อัตโนมัติ!

### ตารางการตรวจจับ Props, Target และ NPC ในไฟล์ DayZ Editor (`.json`)

| โมดูล และโฟลเดอร์วางไฟล์ `.json` | การวาง Props ประกอบฉาก | การวาง Target / จุดสำคัญใน DayZ Editor | การวาง NPC ใน DayZ Editor |
| :--- | :--- | :--- | :--- |
| **1. Contractors**<br>`$profile:\DayZStory\AiBuddy\DLC\Contractors\Scenes\` | โต๊ะ เก้าอี้ เต็นท์ วัตถุทั่วไปและ `.p3d` จะถูกเสกเป็นฉากประกอบ | *(Target ที่กดคุยคือตัว NPC Contractor โดยตรง)* | • **NPC นายหน้า:** วางตัวละครคลาสใดก็ได้ที่สืบทอดจาก `eAIBase` (เช่น `eAI_SurvivorM_Boris`) ระบบจะจับเข้า Faction `eAIFactionContractors`, ตั้งเป็นอมตะ, ยืนประจำจุด และดึง `Loadout` / `EmoteID` / `Name` จากรายการแรก (`Contractors[0]`) ใน `Setting.json` มาใส่ให้อัตโนมัติ |
| **2. Gravedigger**<br>`$profile:\DayZStory\AiBuddy\DLC\Gravedigger\Graves\` | ซุ้มสุสาน รั้ว โคมไฟ ต้นไม้ `.p3d` จะถูกเสกเป็นฉากประกอบ | • **Target หลุมศพ:** วางวัตถุคลาส **`DZS_BuddyGrave`** กี่หลุมก็ได้ในรัศมี `CemeteryRadius` ผู้เล่นสามารถถือพลั่วสัปเหร่อไปกดขุด (`ActionDigBuddyGrave`) ได้ทุกหลุมทันที | • **NPC สัปเหร่อ:** วางตัวละครคลาสใดก็ได้ที่สืบทอดจาก `eAIBase` (เช่น `eAI_SurvivorM_Rolf`) ระบบจะจับเข้า Faction `eAIFactionGravediggers`, ตั้งเป็นอมตะ, ยืนประจำจุด และดึง `Loadout` / `EmoteID` / `Name` จากรายการแรก (`Gravediggers[0]`) ใน `Setting.json` มาใส่ให้อัตโนมัติ (ผู้เล่นกดยืมพลั่วที่ตัว NPC ได้เลย) |
| **3. Satellite**<br>`$profile:\DayZStory\AiBuddy\DLC\Satellite\Terminals\` | เต็นท์สื่อสาร เสาอากาศ วัตถุ `.p3d` จะถูกเสกเป็นฉากประกอบ | • **Target ตู้ดาวเทียม:** วาง **`DZS_SatelliteTerminal`** หรือ **`Land_Radio_PanelPAS`** ระบบจะลงทะเบียนเป็นจุดกดเปิดเมนู Satellite ทันที<br>• **จุดเกิดกล่องเคลมของ (Marker):** วาง **`eAI_SupplyCrate`** ไว้ใกล้ๆ ตู้ดาวเทียม (ในระยะ 50 เมตร) ระบบจะ**ไม่เสกกล่องเปล่าค้างไว้** แต่จะบันทึกพิกัดนั้นเป็น `ClaimBoxPosition` ของตู้ดาวเทียมที่อยู่ใกล้ที่สุดให้อัตโนมัติ! | *(ไม่มี NPC)* |

> [!TIP]
> **ทริคการใช้ไฟล์ DayZ Editor เพียงอย่างเดียวโดยไม่ให้พิกัดใน `Setting.json` เสกซ้ำ:**
> หากคุณวางทั้ง Props, Target และ NPC ไว้ในไฟล์ DayZ Editor ครบแล้ว ให้ตั้งค่า `"Position": [0.0, 0.0, 0.0]` ในรายการ `Contractors[0]` / `Gravediggers[0]` / `Terminals[0]` ของไฟล์ `Setting.json` ได้เลย ระบบจะข้ามไม่เสกพิกัด `[0,0,0]` แต่ยังคงนำค่า `"Loadout"`, `"EmoteID"`, และ `"Name"` จากรายการแรกนั้นไปสวมใส่ให้กับ NPC ที่เสกมาจากไฟล์ DayZ Editor ครับ!
