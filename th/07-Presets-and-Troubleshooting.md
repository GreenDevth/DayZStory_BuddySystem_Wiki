# บทที่ 7: สูตรการตั้งค่าสำเร็จรูปและการแก้ปัญหา (Server Presets & Troubleshooting)

> **[⬅️ บทที่ 6: ระบบ Faction, พันธมิตร และการวางพิกัด NPC](./06-Factions-Coordinates-and-Editor-Guide.md)** | **[🏠 กลับสารบัญภาษาไทย](./README.md)**

---

## 1. สูตรการตั้งค่าสำเร็จรูป 4 สไตล์เซิร์ฟเวอร์ (Copy-Paste Server Presets)

แอดมินสามารถเลือกสูตรที่ตรงกับแนวทางเซิร์ฟเวอร์ แล้วนำค่าไปตั้งในไฟล์ JSON ที่ระบุได้เลยครับ

---

### 🟢 สูตรที่ 1: เซิร์ฟเวอร์แนว PvE Casual / Solo-Friendly (เล่นง่าย ตายแล้วบัดดี้ไม่หาย)
- **แนวคิด:** ผู้เล่นตายแล้วเกิดใหม่สามารถเรียกบัดดี้คนเดิมพร้อมของเดิมออกมาเดินต่อได้เลย ไม่ต้องซื้อประกันเคลมกล่อง และใช้วิทยุเรียกบัดดี้ได้ทันทีโดยไม่ต้องไปจูนคลื่นที่ตู้ดาวเทียม
- **ตั้งค่าใน `BuddySettings.json`:**
  ```json
  "m_EconomyMode": 0,
  "m_DeleteAIOnPlayerDisconnect": 1,
  "m_DeleteAIOnPlayerDeath": 1,
  "m_OwnerDeathBuddyMode": 2,
  "m_EnableRecoverySystem": 0
  ```
- **ตั้งค่าใน `DLC/Radio/Setting.json`:**
  ```json
  "Enable": 1,
  "EnableCallConstructorsMenu": 0,
  "EnableDirectRadioRecall": 1,
  "EnableCallBackCost": 0,
  "RequireRadioPairing": 0
  ```

---

### 🟡 สูตรที่ 2: เซิร์ฟเวอร์แนว PvPvE Tactical Standard (สมดุล — แนะนำเป็นค่ามาตรฐาน)
- **แนวคิด:**
  - ต้องนำวิทยุไปกด **`PAIR RADIO`** ที่ตู้ดาวเทียม 1 ครั้งต่อ 1 ชีวิตตัวละคร
  - ต้องซื้อ **ประกันภัยดิจิทัล (Digital Insurance)** ไว้คุ้มครองของบัดดี้
  - หากบัดดี้ตาย (และมีประกัน) หรือผู้เล่นตาย ของบัดดี้จะถูกล็อกไว้ที่ตู้ดาวเทียม ผู้เล่นต้องไปกดเคลมรับของคืนเป็นกล่อง **`eAI_SupplyCrate`** แล้วค่อยจ้างบัดดี้คนใหม่
- **ตั้งค่าใน `BuddySettings.json`:**
  ```json
  "m_EconomyMode": 0,
  "m_DeleteAIOnPlayerDisconnect": 1,
  "m_DeleteAIOnPlayerDeath": 1,
  "m_OwnerDeathBuddyMode": 0,
  "m_EnableRecoverySystem": 1
  ```
- **ตั้งค่าใน `DLC/Radio/Setting.json`:**
  ```json
  "Enable": 1,
  "EnableDirectRadioRecall": 1,
  "RequireRadioPairing": 1
  ```

---

### 🔴 สูตรที่ 3: เซิร์ฟเวอร์แนว Hardcore Survival & Cemetery Gravedigger (ขุดหลุมศพกู้ของ + เรียกบัดดี้ที่ตู้ดาวเทียมเท่านั้น)
- **แนวคิด:**
  - ปิดการกด Recall บัดดี้ผ่านวิทยุพกพาในป่า (`EnableDirectRadioRecall: 0`) — เวลาเข้าเกมใหม่ผู้เล่นต้องถือวิทยุที่เปิดเครื่องไปกดจูนคลื่นเรียกบัดดี้ที่ **ตู้ดาวเทียม** เท่านั้น!
  - ปิดระบบเคลมกล่องผ่านหน้าจอตู้ดาวเทียม (`m_EnableRecoverySystem: 0`) — เมื่อบัดดี้หรือผู้เล่นตาย ผู้เล่นต้องเดินทางไปที่ **สุสานสัปเหร่อ (`Gravedigger`)** จ่ายค่ายืมพลั่วแล้วลงมือขุดหลุมศพเพื่อเอาของบัดดี้คืน!
- **ตั้งค่าใน `BuddySettings.json`:**
  ```json
  "m_OwnerDeathBuddyMode": 0,
  "m_EnableRecoverySystem": 0
  ```
- **ตั้งค่าใน `DLC/Radio/Setting.json`:**
  ```json
  "Enable": 1,
  "EnableDirectRadioRecall": 0,
  "RequireRadioPairing": 1
  ```
- **ตั้งค่าใน `DLC/Gravedigger/Setting.json`:**
  ```json
  "Enable": 1,
  "RecoveryReputationCost": 1500,
  "ShovelBorrowCost": 250
  ```

---

### 🟣 สูตรที่ 4: เซิร์ฟเวอร์แยกหลายสกุลเงิน (Multi-Currency Economy: Hardline + eWallet / Bank)
- **แนวคิด:**
  - **การจ้างบัดดี้และซื้อวิทยุสื่อสาร:** ใช้แต้มชื่อเสียง **Hardline Reputation (`0`)** เพื่อให้ผู้เล่นต้องออกล่าซอมบี้/ทำภารกิจสะสมชื่อเสียง
  - **การซื้อกรมธรรม์ประกันภัยดิจิทัลและการจ่ายค่าเคลมของ:** ใช้เงิน **eWallet Coins (`1`)** หรือ **เงินธนาคาร Expansion Bank (`2`)** เพื่อระบายเงินในระบบเศรษฐกิจของเซิร์ฟเวอร์
  - *(หน้าหลักของตู้ดาวเทียมจะแสดงยอดเงินทั้ง 2 สกุลคู่กันให้อัตโนมัติ เช่น `48520 Rep  |  15000 Coins`)*
- **ตั้งค่าใน `DLC/Satellite/Setting.json`:**
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

## 2. คู่มือการตรวจสอบและแก้ไขไฟล์เซฟผู้เล่น (`BuddyData/<SteamID64>.json`)

หากผู้เล่นแจ้งปัญหาหรือแอดมินต้องการตรวจสอบข้อมูลบัดดี้ของผู้เล่นรายคน สามารถเปิดไฟล์ได้ที่:
`$profile:\DayZStory\AiBuddy\BuddyData\<SteamID64>.json`

```json
{
    "ClassName": "eAI_SurvivorF_Linda",
    "BuddyName": "Sophia",
    "ProfileID": "linda_allrounder",
    "Faction": "eAIFactionCivilian",
    "OwnerDeadState": 0,
    "RemoveReason": 0,
    "MemberColorARGB": -65424,
    "RadioPaired": 1,
    "Inventory": [ ... ]
}
```

### ความหมายของตัวแปรสถานะในไฟล์เซฟผู้เล่น:
| ชื่อฟิลด์ในไฟล์เซฟ | ค่าที่เป็นไปได้ | ความหมายและวิธีแก้ปัญหาให้ผู้เล่น |
| :--- | :---: | :--- |
| `OwnerDeadState` | `0` หรือ `1` | • `0` = สถานะปกติ ผู้เล่นสามารถกด Recall บัดดี้ตัวนี้ออกมาเดินได้<br>• `1` = **ติดล็อกรอกู้คืนของ** (เพราะเจ้าของตายหรือบัดดี้ตาย) ผู้เล่นต้องไปเคลมกล่องสัมภาระที่ตู้ดาวเทียมหรือขุดหลุมศพก่อน *(หากแอดมินต้องการปลดล็อกให้ผู้เล่นเรียกบัดดี้กลับมาเดินได้ทันที ให้แก้เป็น `0` แล้วเซฟไฟล์)* |
| `RemoveReason` | `0`, `1`, `2`, `3` | สาเหตุล่าสุดที่บัดดี้ถูกเก็บออกจากโลก:<br>• `0` = ผู้เล่นออกเกม / เซิร์ฟรีสตาร์ต (ปกติ)<br>• `1` = ผู้เล่น (เจ้านาย) เสียชีวิต<br>• `2` = บัดดี้ถูกฆ่าตายในสนามรบ (โดยมีประกันคุ้มครอง)<br>• `3` = ผู้เล่นกดสั่งปลดบัดดี้ (Dismiss) |
| `RadioPaired` | `0` หรือ `1` | • `1` = ตัวละครในชีวิตนี้ได้ทำการจูนคลื่นวิทยุ (`PAIR RADIO`) กับตู้ดาวเทียมเรียบร้อยแล้ว<br>• `0` = ยังไม่ได้จูนคลื่นวิทยุ (จะรีเซ็ตเป็น `0` อัตโนมัติเมื่อผู้เล่นตาย) |
| `Faction` | เช่น `"eAIFactionCivilian"` | คลาส Faction ของบัดดี้ที่ซิงค์มาจากผู้เล่นที่จ้าง |

---

## 3. คำถามที่พบบ่อยและวิธีแก้ปัญหา (Troubleshooting FAQ)

### Q1: ทำไมเปิดหน้าตู้ดาวเทียมแล้วปุ่ม `PAIR RADIO` หรือ `RECALL BUDDY` กดไม่ได้ (ขึ้นว่า `HOLD POWERED RADIO`)?
- **สาเหตุ:** ระบบกำหนดให้ผู้เล่นต้อง **ถือวิทยุสื่อสารไว้ในมือ (`In Hands`)** และวิทยุนั้นต้อง **ใส่ถ่าน 9V (`Battery9V`) พร้อมกดเปิดเครื่อง (`Switch On`)** อยู่ด้วย
- **วิธีแก้:** ให้ผู้เล่นถือวิทยุในมือ เลื่อนลูกกลิ้งเมาส์กด **`Switch On`** ก่อน แล้วค่อยกดใช้งานตู้ดาวเทียม (หากซื้อวิทยุจาก `RADIO EQUIPMENT SHOP` ในตู้ดาวเทียม ระบบจะใส่ถ่าน `Battery9V` มาให้พร้อมแล้ว เพียงแค่ถือในมือแล้วกดเปิดเครื่องเท่านั้น)

### Q2: ทำไมตั้งค่าพิกัดตู้ดาวเทียมหรือ NPC สัปเหร่อแล้ว ไม่เห็นมีอะไรเกิดในเกมเลย?
- **สาเหตุที่ 1:** ลืมเปลี่ยนค่า `"Position": [0.0, 0.0, 0.0]` เป็นพิกัดจริง (ระบบจะข้ามรายการที่เป็น `0, 0, 0` เสมอ)
- **สาเหตุที่ 2:** เผลอพิมพ์คอมม่า `,` เกินตัวสุดท้ายในไฟล์ `.json` ทำให้ JSON ผิดไวยากรณ์ (Syntax Error)
- **วิธีตรวจสอบ:** เปิด PowerShell รันคำสั่งตรวจไฟล์ JSON:
  ```powershell
  Get-Content "$profile\DayZStory\AiBuddy\DLC\Satellite\Setting.json" -Raw | ConvertFrom-Json
  ```
  หากไม่มีข้อความสีแดงแจ้งเตือน แปลว่าไฟล์ JSON ถูกต้อง 100%

### Q3: ทำไมบัดดี้ไม่ยอมวิ่งมาปั๊มหัวใจ (CPR) หรือพันแผลให้ผู้เล่น?
- **ตรวจสอบ 4 เงื่อนไขใน `DLC/Medic/Setting.json`:**
  1. หากตั้ง `"OnlyMedicRole": 1` บัดดี้ที่คุณจ้างต้องมีคำว่า `"Medic"` อยู่ในฟิลด์ `"m_Role"` (เช่น `"Field Medic"`) เท่านั้นถึงจะมารักษา (หากอยากให้บัดดี้ทุกคนรักษาได้ ให้ตั้ง `"OnlyMedicRole": 0`)
  2. หากตั้ง `"RequireItems": 1` การพันแผลและดามขาต้องมีผ้าพันแผล (`BandageDressing`/`Rag`) หรือเฝือก (`Splint`) อยู่ใน **กระเป๋าของบัดดี้** (ส่วน CPR ทำได้ฟรีแต่ติด Cooldown `CPRCooldownSeconds`)
  3. บัดดี้ต้องอยู่ห่างจากผู้เล่นไม่เกินระยะ `"MaxAssistDistance"` (ค่าเริ่มต้น `40` เมตร)
  4. บัดดี้ต้องไม่ได้ติดพันการยิงปะทะที่อันตรายเกินค่า `"ThreatThreshold"`

### Q4: ทำไม NPC Contractor หรือ Gravedigger ถึงหันมายิงผู้เล่นในเซฟโซน?
- **สาเหตุ:** ผู้เล่นคนนั้นเผลอยิงปืนใส่บัดดี้หรือทำดาเมจใส่ AI พันธมิตรในระยะสายตาของ NPC ขณะที่ตั้ง `"IsPassive": 0`
- **วิธีแก้:** เปิดไฟล์ `$profile:DayZStory/AiBuddy/DLC/Contractors/Setting.json` และ `DLC/Gravedigger/Setting.json` แล้วเปลี่ยน **`"IsPassive": 1`** เพื่อให้ NPC ยืนสงบ 100% ไม่ยิงตอบโต้ใครในเซฟโซน
