# VTBD Tower · Flight Strips (ออนไลน์)

เกมนี้ใช้ **GitHub Pages** โฮสต์หน้าเว็บ และ **Firebase Realtime Database** ซิงก์ strip / เครื่องบิน / ผู้เล่นออนไลน์แบบเรียลไทม์
ถ้ายังไม่ตั้งค่า Firebase เกมจะเล่นได้แบบคนเดียว (เก็บใน localStorage)

## 1) สร้างโปรเจกต์ Firebase
1. ไปที่ https://console.firebase.google.com → **Add project**
2. เมนู **Build → Realtime Database → Create database**
   - เลือกโลเคชัน **Singapore (asia-southeast1)** (ใกล้ไทยสุด)
   - เลือก **Start in locked mode** (เดี๋ยววางกฎเองในข้อ 3)
3. แท็บ **Rules** → วางเนื้อหาจากไฟล์ `database.rules.json` → **Publish**
4. **Project settings (⚙️) → General → Your apps → ไอคอน `</>` (Web)** → ลงทะเบียนแอป
   แล้วคัดลอกค่า `firebaseConfig`
5. เปิดไฟล์ `firebase-config.js` แล้วแทนที่ค่า `YOUR_...` ทั้งหมด
   (`databaseURL` ดูได้ที่หน้า Realtime Database ด้านบน)

## 2) ขึ้น GitHub Pages
1. สร้าง repo ใหม่บน GitHub (Public) แล้วอัปโหลดไฟล์ทั้งหมดในโฟลเดอร์นี้ (`index.html`, `firebase-config.js`, `.nojekyll`, ...)
   ```bash
   git init && git add . && git commit -m "VTBD online"
   git branch -M main
   git remote add origin https://github.com/<USER>/<REPO>.git
   git push -u origin main
   ```
2. repo → **Settings → Pages → Build and deployment**
   Source: **Deploy from a branch** → Branch: `main` / `(root)` → Save
3. รอ 1–2 นาที จะได้ลิงก์ `https://<USER>.github.io/<REPO>/` ส่งให้เพื่อนเล่นได้เลย

## 3) อนุญาตโดเมนของ Firebase (ถ้าจำเป็น)
Realtime Database ไม่ต้องตั้ง authorized domain ก็ใช้ได้ แต่ถ้าเพิ่ม Auth ภายหลัง
ให้เพิ่ม `<USER>.github.io` ที่ **Authentication → Settings → Authorized domains**

## ทดสอบ
เปิดลิงก์ในสองแท็บ/สองเครื่อง เลือก Sector เดียวกัน (A หรือ B)
มุมขวาล่างต้องขึ้น **● LIVE · ออนไลน์ N** และ strip ที่แก้ต้องเด้งไปอีกเครื่องทันที

## ข้อมูลที่ถูกเก็บ
| path | ใช้ทำอะไร |
|---|---|
| `strips_A`, `strips_B` | flight strip ของแต่ละ sector |
| `meta_*/init` | ธง seed strip ตัวอย่างครั้งแรก |
| `metar_*/cur` | METAR ที่แชร์กัน |
| `presence/sec-a`, `sec-b` | ผู้เล่นออนไลน์ + ตำแหน่งเครื่องบิน (ลบอัตโนมัติเมื่อหลุด) |

## หมายเหตุด้านความปลอดภัย
เกมไม่มีระบบล็อกอิน กฎใน `database.rules.json` จึงเปิดให้ใครก็ตามที่มีลิงก์อ่าน/เขียนได้เฉพาะ path ข้างต้น
เหมาะกับห้องซ้อม/กลุ่มเพื่อน ถ้าจะเปิดสาธารณะจริง แนะนำเพิ่ม Firebase Anonymous Auth
และตั้ง Budget alert ใน Google Cloud เพื่อกันการใช้งานเกินโควตาฟรี

## รีเซ็ตข้อมูล
Firebase Console → Realtime Database → Data → ลบ node `strips_A` / `strips_B` / `meta_*`
เกมจะ seed strip ตัวอย่างใหม่ให้เองเมื่อเปิดครั้งถัดไป
