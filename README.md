# จัดคู่ตีแบด

หน้าเว็บจัดคิวแมตช์แบดมินตันประเภทคู่ เป็นไฟล์เดียว (`index.html`)

- ถ้ายังไม่ได้ตั้งค่า Firebase ข้อมูลจะเก็บในเบราว์เซอร์ของเครื่องที่เปิดเท่านั้น
- ถ้าตั้งค่า Firebase แล้ว ผู้จัดจะส่งลิงก์ให้เพื่อนเปิดดูคิวแบบอัปเดตสดได้ เพื่อนดูได้อย่างเดียว แก้ไม่ได้

## 1. สร้างฐานข้อมูล Firebase (ฟรี)

1. เข้า https://console.firebase.google.com แล้วกด **Create a project** ตั้งชื่ออะไรก็ได้ เช่น `badminton` (ไม่ต้องเปิด Google Analytics)
2. เมนูซ้าย **Build → Realtime Database** แล้วกด **Create Database**
   - Location: เลือก **Singapore (asia-southeast1)**
   - Security rules: เลือก **Start in locked mode** แล้วกด Enable
3. ไปที่แท็บ **Rules** วางกฎนี้แทนของเดิม แล้วกด **Publish**

   ```json
   {
     "rules": {
       "rooms": {
         "$room": {
           ".read": true,
           ".write": true
         }
       }
     }
   }
   ```

4. กดรูปเฟืองข้าง Project Overview → **Project settings** → เลื่อนลงไปที่ **Your apps** → กดไอคอน `</>` (Web) ตั้งชื่อแอป แล้วกด Register app
5. จะมีโค้ด `const firebaseConfig = { ... }` ขึ้นมา ให้คัดลอกค่าในวงเล็บปีกกาเก็บไว้ ถ้าไม่มีบรรทัด `databaseURL` ให้ไปคัดลอกจากหน้า Realtime Database ด้านบนสุด (หน้าตาแบบ `https://badminton-xxxx-default-rtdb.asia-southeast1.firebasedatabase.app`)

## 2. ใส่ค่าใน index.html

เปิด `index.html` บน GitHub แล้วกดรูปดินสอเพื่อแก้ไข หาบรรทัด `const FIREBASE_CONFIG = {` แล้วใส่ค่าที่คัดลอกมา:

```js
const FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "badminton-xxxx.firebaseapp.com",
  databaseURL: "https://badminton-xxxx-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "badminton-xxxx",
  appId: "1:1234:web:abcd"
};
```

กด **Commit changes** แล้วรอ GitHub Pages deploy ใหม่ประมาณ 1 นาที

`apiKey` ของ Firebase ไม่ใช่รหัสลับ ใส่ในไฟล์ที่เปิดให้คนทั่วไปเห็นได้ สิ่งที่คุมสิทธิ์คือ Rules ในข้อ 3

## 3. ใช้งาน

1. ผู้จัดเปิดลิงก์ GitHub Pages ตามปกติ ที่อยู่เว็บจะมี `?room=xxxx` ต่อท้ายให้เอง และด้านบนจะขึ้นว่า "ออนไลน์"
2. กด **คัดลอกลิงก์ให้เพื่อนดู** แล้ววางในกลุ่ม LINE เพื่อนจะเห็นคิว ใครกำลังเล่น และใครลงแมตช์ถัดไป อัปเดตทันทีที่ผู้จัดกด "จบแล้ว"
3. ถ้าผู้จัดจะเปลี่ยนไปใช้เครื่องอื่น กด **ลิงก์ผู้จัด** แล้วเปิดลิงก์นั้นในเครื่องใหม่

ครั้งต่อไปให้เปิดลิงก์ผู้จัดเดิมแล้วกด "เริ่มวันใหม่" เพื่อนจะใช้ลิงก์ดูลิงก์เดิมต่อได้

## หมายเหตุ

- ให้แก้คิวจากเครื่องเดียว ถ้าสองเครื่องแก้พร้อมกัน ค่าที่บันทึกทีหลังจะทับค่าก่อนหน้า
- ลิงก์ให้เพื่อนดูซ่อนปุ่มแก้ไขไว้เท่านั้น ใครที่รู้รหัสห้องและตั้งใจเขียนโค้ดเอง ก็เขียนข้อมูลลงห้องนั้นได้ เหมาะกับใช้ในก๊วนเพื่อน ไม่เหมาะกับข้อมูลสำคัญ
