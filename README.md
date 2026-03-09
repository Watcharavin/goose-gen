# 🪿 Goose Gen

> AI-powered brand image generation request form — ส่งข้อมูลแบรนด์เข้า n8n workflow ได้ในคลิกเดียว

---

## 📋 Project Overview

Goose Gen เป็นเว็บฟอร์มสำหรับรวบรวมข้อมูลแบรนด์และ campaign แล้วส่งไปยัง n8n webhook
เพื่อ trigger workflow สร้างภาพอัตโนมัติ ไม่มี database — pure frontend form submission

**Tech Stack:** Single HTML file · HTML + CSS + Vanilla JS · LocalStorage · Fetch API

---

## 🗺️ Pages & Flow

```
[Page 1 — Landing]
  ↓ กดปุ่ม "เริ่มต้นเลย →"
[Page 2 — Form]
  ↓ กรอกครบแล้วกด "ส่งข้อมูล →"
[POST → n8n Webhook]
  ↓ รับ response
[Success Modal Popup]
  → "ส่งข้อมูลเรียบร้อย รอรับผลที่ email ของคุณ"
```

---

## 📄 Page 1 — Landing

- Full viewport hero
- ชื่อเว็บ **"Goose Gen"** ใน Caveat font ขนาดใหญ่
- Tagline: _"สร้างภาพแบรนด์ของคุณ ง่ายกว่าที่คิด"_
- ปุ่ม CTA: **"เริ่มต้นเลย →"** → ไปหน้า Form
- Decorative ✦ stars และ wavy background elements

---

## 📝 Page 2 — Form

Layout: single scrollable column · max-width 780px · centered
ทุก section เป็น card (rounded corners, cream-dark background)

### Card Section 1 — Biodata
| Field | Type |
|-------|------|
| Brand Name | Text input |
| CI Color 1–3 | Color picker × 3 (แสดง hex value) |
| Link Picture Logo | URL input |

### Card Section 2 — Ref Pic
| Field | Type |
|-------|------|
| Image Link 1–3 | URL input × 3 |

### Card Section 3 — Voucher
| Field | Type |
|-------|------|
| Title | Text input |
| Detail | Textarea |
| Image Link 1–5 | URL input × 5 |

### Card Section 4 — Collection
| Field | Type |
|-------|------|
| Title | Text input |
| Detail | Textarea |
| Image Link 1–10 | URL input × 10 |

### Card Section 5 — Coupons (Dynamic)
- ปุ่ม **"Add Coupon"** เพิ่มได้สูงสุด **30 coupons**
- แสดงผลเป็น **horizontal carousel** เลื่อนซ้าย→ขวา (CSS scroll-snap)
- ชื่อ card: "Coupon 1", "Coupon 2", ... ตามลำดับ
- แต่ละ coupon card มี delete button

| Field | Type |
|-------|------|
| Title | Text input |
| Type | Dropdown: รับฟรี / 1แถม1 / ส่วนลด บาท / ส่วนลด เปอร์เซนต์ / Birthday / Double คุ้ม |
| Detail | Textarea |
| Image Link 1–5 | URL input × 5 |

### Card Section 6 — ภาพบรรยากาศ (Dynamic)
- ปุ่ม **"Add ภาพบรรยากาศ"** เพิ่มได้สูงสุด **5 cards**
- แสดงผลเป็น **horizontal carousel** เลื่อนซ้าย→ขวา (CSS scroll-snap)
- ชื่อ card: "ภาพบรรยากาศ 1", "ภาพบรรยากาศ 2", ... ตามลำดับ
- แต่ละ card มี delete button

| Field | Type |
|-------|------|
| Title | Text input |
| Detail | Textarea |
| Image Link 1–5 | URL input × 5 |

### Card Section 7 — Rich Menu
| Field | Type |
|-------|------|
| Image Link | URL input × 1 |

### Section 8 — Email
| Field | Type |
|-------|------|
| Email | Email input (พร้อม validation) |

---

## 📦 JSON Payload (ส่งไปยัง n8n)

```json
{
  "biodata": {
    "brandName": "",
    "ciColors": ["#hex1", "#hex2", "#hex3"],
    "logoLink": ""
  },
  "refPics": ["link1", "link2", "link3"],
  "voucher": {
    "title": "",
    "detail": "",
    "images": ["link1", "link2", "link3", "link4", "link5"]
  },
  "collection": {
    "title": "",
    "detail": "",
    "images": ["link1", "...", "link10"]
  },
  "coupons": [
    {
      "title": "",
      "type": "",
      "detail": "",
      "images": ["link1", "...", "link5"]
    }
  ],
  "atmosphere": [
    {
      "title": "",
      "detail": "",
      "images": ["link1", "...", "link5"]
    }
  ],
  "richMenu": {
    "imageLink": ""
  },
  "email": ""
}
```

> ⚙️ กำหนด webhook URL ที่ตัวแปร `WEBHOOK_URL` ด้านบนของ JS:
> ```js
> const WEBHOOK_URL = "https://your-n8n-webhook-url.com/webhook/goose-gen";
> ```

---

## 💾 LocalStorage Auto-save

- **Auto-save** ทุก input change → key: `gooseGenFormData`
- **Auto-restore** เมื่อ reload หน้า (รวมถึง dynamic cards/coupons)
- **Clear** หลัง submit สำเร็จ
- แสดง indicator: _"✦ บันทึกอัตโนมัติแล้ว"_ เบาๆ มุมล่าง

---

## 🎨 Design System

### Color Palette
| Role | Hex | Usage |
|------|-----|-------|
| Background | `#FAF3E3` | พื้นหลักทั้งเว็บ (warm cream) |
| Background Dark | `#F0E6CC` | Card background |
| Primary Accent | `#C25048` | Terracotta red — labels, bullets, accents |
| Secondary Accent | `#485CC2` | Cornflower blue — CTA buttons, focus states |
| Text Dark | `#1A1410` | Warm near-black — headings, body |
| Text Muted | `#5A4F40` | Secondary text |

### Typography
| Role | Font | Weight |
|------|------|--------|
| Logo / Brand | Caveat | Bold, handwritten feel |
| Headings | Playfair Display | 700–900 |
| Heading Accent | Playfair Display Italic | ใน #485CC2 หรือ #C25048 |
| Body / UI | DM Sans | 300–500 |
| Labels / Tags | DM Sans Uppercase | letter-spacing 0.12–0.15em |

> Import from Google Fonts:
> ```
> Caveat:wght@700 · Playfair+Display:ital,wght@0,700;0,900;1,700 · DM+Sans:wght@300;400;500
> ```

### Components
- **Buttons:** pill-shaped `border-radius: 999px` · ALL CAPS · wide letter-spacing
  - Outline: dark border + transparent bg
  - Filled: `#485CC2` bg + flat offset shadow `4px 4px 0`
- **Cards:** `border-radius: 20–28px` · hover `translateY(-4px)`
- **Shadows:** flat offset `8px 8px 0 rgba(0,0,0,0.08)` — ไม่ใช้ gaussian blur
- **Inputs:** `border-radius: 12px` · focus border `#485CC2`
- **Dividers:** wavy SVG paths ระหว่าง section
- **Decorative:** ✦ stars · opacity 0.3–0.5

### Vibe
> Warm retro-groovy meets modern editorial.
> Approachable creative-professional — ไม่ corporate ไม่ childish

---

## 📁 File Structure

```
goose-gen/
└── index.html      ← ไฟล์เดียว (HTML + CSS + JS รวมกัน)
```

---

## 🚀 Getting Started

1. เปิด `index.html` ในเบราว์เซอร์
2. แก้ `WEBHOOK_URL` ใน JS ให้ตรงกับ n8n webhook ของคุณ
3. Deploy บน static hosting ใดก็ได้ (Netlify, Vercel, GitHub Pages)

---

## ✦ Built with Goose Gen · Powered by n8n