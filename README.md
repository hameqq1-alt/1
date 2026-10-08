# ระบบเงินเดือน (Google Apps Script)

ระบบเงินเดือนสำหรับพนักงานรายชั่วโมง/Part-time บน Google Apps Script + Google Sheet + หน้าเว็บ
ไม่มีชื่อบริษัทหรือโลโก้ฝังในโค้ด — ตั้งเองในเมนู **ตั้งค่าระบบ** หลังติดตั้ง

| ไฟล์ | หน้าที่ |
|---|---|
| `Code.gs` | ฝั่งเซิร์ฟเวอร์: ชีต/โฟลเดอร์, API หน้าเว็บ, คำนวณเงิน, สลิป, โลโก้ |
| `Index.html` | หน้าเว็บทั้งระบบ (16 เมนู) |
| `TimesheetOcr.gs` | AI (Gemini) อ่านรูปใบลงเวลาฟอร์ม `PAY-TS` — ต้องตั้ง Script Property `GEMINI_API_KEY` |
| `appsscript.json` · `.claspignore` | manifest (Asia/Bangkok, V8) · อัปขึ้น Google เฉพาะ 4 ไฟล์ |
| `tools/make-preview.js` | `node tools/make-preview.js` → เปิด `preview.html` ดูหน้าเว็บด้วยข้อมูลสมมติ |

## ติดตั้ง
```bash
cp appsscript.json appsscript.json.bak               # create-script เขียนทับ manifest
npx clasp create-script --type standalone --title "ระบบเงินเดือน"
cp appsscript.json.bak appsscript.json
npx clasp push -f                                    # ต้องเห็น "Pushed 4 files"
npx clasp create-deployment -d "ติดตั้งครั้งแรก"       # เก็บ deploymentId ไว้ redeploy ด้วย -i
```
1. เจ้าของบัญชีเปิดเอดิเตอร์ → Run `setup` → อนุญาตสิทธิ์ (ก่อนนี้ /exec ตอบ 403)
2. เปิด URL /exec → หน้า **ตั้งค่าระบบ**: อัปโลโก้ · ข้อมูลบริษัท · กติกาคำนวณ (ชม.ปกติ, ตัวคูณ OT, พัก, OT ขั้นต่ำ)
3. ตำแหน่งงาน (ค่าแรง/ชม.) → พนักงาน → พิมพ์ใบลงเวลา
4. (ถ้าใช้ AI) สร้างคีย์ที่ https://aistudio.google.com/apikey ในโปรเจกต์ใหม่ → Script Properties `GEMINI_API_KEY` → ตั้งค่าระบบ → กด **ทดสอบคีย์ AI** ต้องขึ้น ✓
5. (ถ้าใช้บอท LINE) Run `setupBotSecret` แล้วเอารหัสจาก Execution log ไปตั้งฝั่งบอท
