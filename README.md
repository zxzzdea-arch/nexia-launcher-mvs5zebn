# Nexia Launcher 0.5.1

Minecraft Java Edition Launcher สำหรับ Windows x64 ธีมดำขาว ใช้ Discord ตรวจสิทธิ์เข้าถึง Instance และใช้ Microsoft Authentication สำหรับบัญชี Minecraft แยกจาก Discord โดยไม่ต้องติดตั้ง Minecraft Launcher ทางการ

## หน้าหลักและ Instance
- หน้าแรกแสดงปก Instance แบบการ์ด คลิกการ์ดเพื่อเปิดหน้า Info / Mods / Resource Packs / Shaders
- ลากรูป PNG, JPG หรือ WEBP ลงบนการ์ด Instance หรือแถบปกในหน้ารายละเอียดเพื่อเปลี่ยนปก รูปจะถูกย่อให้เหมาะกับ UI และเก็บใน `%APPDATA%\.nexia\artwork`
- ปุ่ม `>_` เปิด Console / Startup Log แสดงขั้นตอนล็อกอิน Microsoft ดาวน์โหลดไฟล์เกม และเปิดเกมโดยตรง พร้อมตาม `logs/latest.log` ระหว่างที่เกมทำงาน คัดลอก Log ได้
- ตั้ง RAM ต่อ Instance จากปุ่มเฟือง โดยคำนวณเพดานจาก RAM เครื่องและส่งไปยังโปรเซส Minecraft เมื่อกด PLAY
- Instance ที่สร้างเองลบได้จาก Settings ของ Instance หรือปุ่มลบในหน้า Library การลบเอารายการออกและล้างโปรไฟล์เก่าถ้ามี โดยไม่ลบโฟลเดอร์เกมหรือโลกในกรณีลบ Instance ที่สร้างเอง
- PLAY ตรวจสอบสิทธิ์บัญชี Microsoft, เตรียม Minecraft/Loader/Java, ดาวน์โหลด Assets และ Libraries ที่ขาด แล้วเปิดเกมโดยตรงผ่าน Nexia ไม่จำเป็นต้องติดตั้ง Minecraft Launcher ทางการ; ผู้เล่นต้องใช้บัญชี Microsoft ที่มีสิทธิ์เล่น Minecraft: Java Edition

## สร้าง Instance เอง
กด `+ สร้าง Instance` → ตั้งชื่อ, เลือกเวอร์ชัน Minecraft, Loader (Fabric / Forge / Vanilla), เวอร์ชัน Loader และ RAM ระบบจะสร้าง Instance ใน `%APPDATA%\.nexia\instances\custom-xxxxxxxx` และเก็บข้อมูลเวอร์ชัน/Java ไว้ใน `.nexia-launch.json` ภายใน Instance

## Library (Modrinth เท่านั้น)
- ค้นหา Mods / Libraries, Resource Packs และ Shaders จาก Modrinth API
- แสดงได้สูงสุด 100 รายการต่อหน้า และกด `โหลดเพิ่มอีก 100 รายการ` เพื่อดูผลลัพธ์ถัดไป
- เลือก Instance ที่ติดตั้งแล้วเพื่อดาวน์โหลดรายการลง `mods`, `resourcepacks` หรือ `shaderpacks` โดยไม่ต้องมี `launcher_profiles.json`
- ตัวติดตั้งเลือกเวอร์ชันให้ตรงกับ Minecraft/Loader ของ Instance และติดตั้ง Required Dependencies ที่ Modrinth ระบุไว้ โดยตั้งขีดจำกัดป้องกัน dependency loop หรือดาวน์โหลดไฟล์จำนวนมากผิดปกติ

## ตั้งค่า Discord OAuth
1. เปิด Discord Developer Portal → สร้าง Application
2. OAuth2 → Redirects เพิ่ม `http://127.0.0.1:53682/callback`
3. ใส่ Client ID ใน `config.json` เป็น `discordClientId`
4. ใส่ backend URL ที่ `apiBase` ตามระบบเซิร์ฟเวอร์ของคุณ

Backend เรียก `POST {apiBase}/api/instances` พร้อม `Authorization: Bearer <Discord access token>` และตอบ `{ "whitelisted": true, "instances": [{"id":"...","name":"...","mc":"1.20.1","loader":"forge","loaderVersion":"...","packUrl":"...","ramMb":4096,"description":"...","coverUrl":"https://...","bannerUrl":"https://...","iconUrl":"https://..."}], "launcher": {"version":"...","url":"https://...","notes":"..."} }`

## Build
```bash
npm install
npm run start
```

สร้างตัวติดตั้ง Windows:
```bash
npm run dist
```
ผลลัพธ์อยู่ใน `dist/Nexia-Launcher_<version>_x64-setup.exe`

## หมายเหตุ
- บัญชี Discord ใช้ตรวจสิทธิ์เข้าใช้ Nexia/รายการ Instance เท่านั้น ส่วนบัญชี Microsoft ใช้ยืนยันสิทธิ์เล่น Minecraft Java Edition
- Token Microsoft จะบันทึกด้วย Electron safeStorage เมื่อระบบเข้ารหัสของ Windows พร้อมใช้งาน; ปุ่ม “เปลี่ยนบัญชี Microsoft” ใน Console จะลบเซสชันที่บันทึกไว้
- Java, Instance และไฟล์ Mod เก็บที่ `%APPDATA%\.nexia`; ไฟล์ Minecraft / Libraries / Assets เก็บที่ `%APPDATA%\.minecraft` โดย Nexia ไม่จำเป็นต้องเปิด Minecraft Launcher ทางการ
- Instance ที่มาจากเซิร์ฟเวอร์เป็นรายการที่จัดการโดย backend จึงลบออกจากรายการเซิร์ฟเวอร์ไม่ได้ แต่สามารถลบไฟล์ติดตั้งในเครื่องผ่าน Library หรือ Settings ได้ ซึ่งจะลบมอด/คอนฟิก/โลกในโฟลเดอร์ Instance ด้วย
