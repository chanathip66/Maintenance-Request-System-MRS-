# Maintenance Request System (MRS)

ระบบแจ้งซ่อมอุปกรณ์สำนักงานภายในองค์กร สำหรับรับแจ้งปัญหา ติดตามสถานะ และจัดการงานซ่อมระหว่างผู้ใช้งานกับฝ่าย IT หรือฝ่ายอาคารสถานที่

ตัวระบบออกแบบในรูปแบบ Editorial / Paper-like ให้มีลักษณะคล้ายใบแจ้งซ่อมและสมุดทะเบียนงานจริง พร้อมรองรับ Light/Dark mode และการใช้งานบนคอมพิวเตอร์ แท็บเล็ต และโทรศัพท์มือถือ

## ภาพตัวอย่าง

### หน้าเข้าสู่ระบบ

ผู้ใช้งานสามารถเข้าสู่ระบบด้วยชื่อผู้ใช้และรหัสผ่าน หรือเลือกเข้าสู่ช่องทางสำหรับผู้ดูแลระบบ

![หน้าเข้าสู่ระบบ](https://github.com/user-attachments/assets/d1a88c2d-bb99-4d68-8471-0e3e4f84da95)

### หน้าลงทะเบียน

ผู้แจ้งซ่อมต้องลงทะเบียนก่อนใช้งาน โดยระบุชื่อผู้ใช้ รหัสผ่าน ชื่อ-นามสกุล แผนก และอีเมล

![หน้าลงทะเบียน](https://github.com/user-attachments/assets/187f622d-5bbd-4a4c-be38-70b28fb15abf)

### หน้าผู้ดูแลระบบ

ผู้ดูแลสามารถดูใบแจ้งซ่อมทั้งหมด กรองข้อมูลตามสถานะ ประเภทอุปกรณ์ และแผนก รวมถึงมอบหมายผู้รับผิดชอบและปรับสถานะงาน

![หน้าผู้ดูแลระบบ](https://github.com/user-attachments/assets/2fd24d50-c98e-46b5-bfd0-0f73ad39506e)

### หน้าแจ้งซ่อม

ผู้ใช้งานสามารถสร้างใบแจ้งซ่อม ระบุอุปกรณ์ อาการเสีย สถานที่ ระดับความเร่งด่วน และแนบรูปภาพประกอบ

![หน้าแจ้งซ่อม](https://github.com/user-attachments/assets/cd49c0e9-fd0a-4059-93b3-6da46f977f2d)

## ความสามารถหลัก

- ลงทะเบียนและเข้าสู่ระบบสำหรับผู้แจ้งซ่อม
- ช่องทางเข้าสู่ระบบแยกสำหรับผู้ดูแล
- สร้างและติดตามใบแจ้งซ่อมของตนเอง
- ตารางทะเบียนงานซ่อมพร้อมตัวกรอง
- สถานะงาน: รอรับเรื่อง, กำลังดำเนินการ, ซ่อมเสร็จแล้ว และซ่อมไม่ได้/ส่งเคลม
- มอบหมายช่างและบันทึกผลการดำเนินการ
- แสดงลำดับเหตุการณ์ของใบงานในรูปแบบ Timeline
- แนบรูปภาพ บีบอัดก่อนส่ง และแสดงภาพตัวอย่าง
- Skeleton Loader ระหว่างโหลดข้อมูลจากคลาวด์
- Light/Dark mode
- Responsive design รองรับหน้าจอทุกขนาด
- เชื่อมต่อ Google Apps Script ด้วย Fetch API
- จัดเก็บข้อมูลใน Google Sheets และรูปภาพใน Google Drive

## บทบาทผู้ใช้งาน

| บทบาท | สิทธิ์การใช้งาน |
|---|---|
| ผู้แจ้งซ่อม | ลงทะเบียน, เข้าสู่ระบบ, สร้างใบแจ้งซ่อม และดูรายการของตนเอง |
| ผู้ดูแลระบบ | ดูรายการทั้งหมด, กรองงาน, มอบหมายผู้รับผิดชอบ, เพิ่มบันทึก และเปลี่ยนสถานะ |

## เทคโนโลยี

- HTML5
- Tailwind CSS ผ่าน CDN
- Vanilla JavaScript
- Fetch API
- Google Apps Script Web App
- Google Sheets
- Google Drive
- IBM Plex Serif, IBM Plex Sans Thai และ IBM Plex Mono

## โครงสร้างข้อมูล Google Sheets

สร้าง Spreadsheet ที่มีแผ่นงานและหัวตารางตรงตามรายการต่อไปนี้

### `Tickets`

```text
TicketID, Timestamp, Username, ReporterName, Department, ReporterEmail,
DeviceType, Problem, Urgency, Status, Technician, StartedAt, CompletedAt, Note
```

### `KeyValue`

```text
Key, Value
```

ตัวอย่างข้อมูลเริ่มต้น:

| Key | Value |
|---|---:|
| LastTicketNo | 0 |

### `Users`

```text
Username, Password, FullName, Department, Email, RegisteredAt
```

## การติดตั้ง Backend

1. เปิด Google Spreadsheet ที่ต้องการใช้เป็นฐานข้อมูล
2. เลือก `Extensions > Apps Script`
3. นำสคริปต์ `Code.gs` ของระบบไปวางใน Apps Script Editor
4. รันฟังก์ชัน `setupSheets()` หนึ่งครั้ง เพื่อสร้างและตรวจสอบโครงสร้างชีต
5. เปิด `Project Settings > Script Properties`
6. เพิ่ม `ADMIN_CODE` และกำหนดรหัสผู้ดูแลที่คาดเดาได้ยาก
7. หากเป็น Standalone Script ให้เพิ่ม `SPREADSHEET_ID` เป็น ID ของ Spreadsheet
8. เลือก `Deploy > New deployment > Web app`
9. ตั้งค่า `Execute as: Me` และ `Who has access: Anyone`
10. คัดลอก URL ที่ลงท้ายด้วย `/exec` ไปกำหนดในตัวแปร `API_URL`

เมื่อแก้ไข `Code.gs` ต้องเข้า `Deploy > Manage deployments` และสร้างเวอร์ชันใหม่ทุกครั้ง มิฉะนั้น Web App จะยังรันโค้ดเวอร์ชันเดิม

## การเปิดใช้งาน Frontend

โปรเจกต์ใน repository ใช้ `index.html` แบบ standalone โดยรวม JavaScript ที่จำเป็นไว้ภายในไฟล์แล้ว สามารถเปิดไฟล์โดยตรง หรือเผยแพร่ผ่าน GitHub Pages ได้

การเปิด GitHub Pages:

1. ไปที่ `Settings > Pages`
2. เลือก `Deploy from a branch`
3. เลือก branch `main` และโฟลเดอร์ `/ (root)`
4. กด `Save`

## การเรียก API

Google Apps Script Web App ไม่รองรับ CORS preflight แบบทั่วไป คำขอ POST จึงใช้ `text/plain;charset=utf-8` แล้วส่งข้อมูล JSON ใน body

```js
const response = await fetch(API_URL, {
  method: 'POST',
  headers: {
    Accept: 'application/json',
    'Content-Type': 'text/plain;charset=utf-8'
  },
  body: JSON.stringify({
    action: 'login',
    username: 'username',
    password: 'password'
  })
});

const result = await response.json();
```

คำสั่งหลักที่ API รองรับ:

| Method | Action / Resource | รายละเอียด |
|---|---|---|
| POST | `register` | ลงทะเบียนผู้ใช้งาน |
| POST | `login` | เข้าสู่ระบบผู้แจ้งซ่อม |
| POST | `adminLogin` | เข้าสู่ระบบผู้ดูแล |
| POST | `createTicket` | สร้างใบแจ้งซ่อม |
| POST | `updateTicket` | แก้ไขสถานะและข้อมูลใบงาน |
| POST | `logout` | ยกเลิก Session |
| GET | `tickets` | อ่านรายการใบแจ้งซ่อม |
| GET | `ticket` | อ่านรายละเอียดใบงาน |
| GET | `attachment` | โหลดรูปภาพแนบโดยตรวจสอบสิทธิ์ |

## หมายเหตุด้านความปลอดภัย

- ไม่ควรเขียนรหัสผู้ดูแลไว้ใน Frontend หรือ README
- ควรกำหนดรหัสผู้ดูแลผ่าน Script Property ชื่อ `ADMIN_CODE`
- ผู้ใช้ทั่วไปอ่านได้เฉพาะใบแจ้งซ่อมของตนเอง
- API จะไม่ส่งคอลัมน์รหัสผ่านกลับมายัง Frontend
- รูปภาพใน Google Drive ถูกโหลดผ่าน API หลังตรวจสอบ Session และสิทธิ์ของใบงาน
- ควรเปลี่ยน Deployment URL หาก URL เดิมถูกเผยแพร่โดยไม่ตั้งใจ

## ผู้พัฒนา

พัฒนาโดย [chanathip66](https://github.com/chanathip66)

